# React

* [Rendering Process](#rendering-process)
* [Reconciliation](#reconciliation)
* [Life-Cycle](#life-cycle)
* [Hooks](#hooks)
* [Performance Optimization](#performance-optimization)

## Rendering Process
- React의 Rendering Process는 Render Phase와 Commit Phase로 이루어짐
- Render: 새 가상 DOM을 생성하고 이전 가상 DOM이 있다면 새 가상 DOM과 비교하는 단계
    1. React App이 최초 실행된 경우
        1. Component가 parsing되고 JSX가 React.createElement를 통해 React Element로 변환되고 메모리에 저장
        1. React Element를 통해 새 가상 DOM을 생성
    1. state 또는 props가 업데이트된 경우
        1. 상태 변경을 트리거한 Component에 플래그를 지정
        1. Component와 하위 Component들이 parsing되고 JSX가 React.createElement를 통해 React Element로 변환되고 메모리에 저장
        1. React Element를 통해 새 가상 DOM을 생성
        1. diffing 알고리즘을 통해 이전 가상 DOM과 새 가상 DOM을 비교
- Commit: 새 가상 DOM 또는 변경 사항을 실제 DOM에 업데이트하는 단계
    1. React DOM 라이브러리를 사용하여 새 가상 DOM 또는 변경 사항을 실제 DOM에 업데이트

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react)
## Reconciliation
- Reconciliation이란 이전 가상 DOM과 새 가상 DOM을 비교하고 변경 사항을 실제 DOM에 업데이트하는 과정 (Render + Commit)
- React는 이전 가상 DOM과 새 가상 DOM을 비교하기 위해 아래 2가지 가정을 기반한 시간 복잡도 O(n)의 Diffing 휴리스틱 알고리즘을 사용
    1. 서로 다른 타입의 두 Elements는 서로 다른 Tree를 만들어낸다.
    1. 개발자가 key prop을 통해, 여러 렌더링 사이에서 어떤 자식 Element가 변경되지 않아야 할 지 표시해 줄 수 있다.
- 비교 알고리즘 (Diffing Algorithm)
    - React가 두 개의 트리를 비교하는 시기: **state 또는 props가 업데이트**됐을 때
    - 가장 먼저 비교하는 것: 두 개의 **Root Elements**
    1. 다른 타입의 Elements
        - 두 Root Elements의 타입이 다르면, React는 이전 Tree를 버리고 완전히 새로운 Tree를 구축
            ```tsx
            import { useState, useEffect } from 'react';

            const App = (): JSX.Element => {
                const [tagName, setTagName] = useState<string>('div');
                const TagName = tagName as keyof JSX.IntrinsicElements;

                return (
                    <TagName>
                        <button onClick={() => {
                            setTagName('article');
                        }}>
                            Change Tag Name to destroy ChildComponent
                        </button>
                        <ChildComponent />
                    </TagName>
                );
            };

            const ChildComponent = (): JSX.Element => {
                useEffect(() => {
                    console.log('ChildComponent --> render');

                    return (): void => {
                        console.log('ChildComponent --> unMount');
                    };
                }, []);

                return <div></div>;
            };

            export default App;
            ```
    1. 같은 타입의 DOM Elements
        - 같은 타입의 DOM Elements를 비교할 때, React는 두 Elements의 속성을 확인하여, 동일한 내역은 유지하고 변경된 속성들만 갱신
        - DOM 노드의 처리가 끝나면, React는 이어서 해당 노드의 자식들을 재귀적으로 처리
            ```tsx
            import { useState, useEffect } from 'react';

            const App = (): JSX.Element => {
                const [isBefore, setIsBefore] = useState<boolean>(true);

                return (
                    <div
                        className={isBefore ? 'before' : 'after'}
                        style={{ color: isBefore ? 'red' : 'green', fontWeight: 'bold' }}
                        title='stuff'
                    >
                        <button onClick={() => {
                            setIsBefore(false);
                        }}>Press me to update attributes</button>
                        <ChildComponent />
                    </div>
                );
            };

            const ChildComponent = (): JSX.Element => {
                useEffect(() => {
                    console.log('ChildComponent --> render');

                    return (): void => {
                        console.log('ChildComponent --> unMount');
                    };
                }, []);

                return <div></div>;
            };
            
            export default App;
            ```
    1. 같은 타입의 Component Elements
        - Component가 갱신되면 인스턴스는 동일하게 유지되어 렌더링 간 state가 유지됨. React는 새로운 Element의 내용을 반영하기 위해 현재 Component 인스턴스의 props를 갱신함. 이때 해당 인스턴스의 UNSAFE_componentWillReceiveProps(), UNSAFE_componentWillUpdate(), componentDidUpdate를 호출
        - 다음으로 render() 메서드가 호출되고 비교 알고리즘이 이전 결과와 새로운 결과를 재귀적으로 처리
    1. 자식에 대한 재귀적 처리
        - DOM 노드의 자식들을 재귀적으로 처리할 때, React는 기본적으로 **동시에 두 리스트를 순회하고 차이점이 있으면 변경을 생성**
        - 자식들이 key를 가지고 있다면, React는 key를 통해 기존 Tree와 이후 Tree의 자식들이 일치하는지 확인함. 예를 들어, 아래 예시에 key를 추가하여 Tree의 변환 작업이 효율적으로 수행되도록 수정할 수 있음
            ```tsx
            import { useState, useEffect } from 'react';

            const App = (): JSX.Element => {
                const [counts, updateCount] = useState<number[]>([1, 2, 3, 4, 5, 6, 7, 8, 9]);
                
                return (
                    <>
                        <button onClick={() => updateCount([counts.length + 1, ...counts])}>
                            Press me
                        </button>
                        <ul>
                            {counts.map((item: number) => (
                                <ChildComponent key={item} text={item} />
                            ))}
                        </ul>
                    </>
                );
            };

            const ChildComponent = (props: { text: number }): JSX.Element => {
                const [state] = useState<number>(props.text);

                return (
                    <li>
                        state: {state} -- text props: {props.text}
                    </li>
                );
            };

            export default App;
            ```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react)
## Life-Cycle
- 순서: Mount -> Update -> Unmount
- 상태
    - Mount
        - Component가 렌더링되어 DOM을 조작할 수 있는 상태
        - Functional Component에서 아래와 같이 구현. but, useEffect는 매 렌더링마다 동기화를 하는 개념으로 이해하면 좋다.
            ```tsx
            useEffect(() => {
                // You can control DOM elements
            }, []);
            ```
    - Update
        - Component 내 props나 state값이 변경되면서 Component가 재렌더링되어 변경된 DOM을 조작할 수 있는 상태
        - Functional Component에서 아래와 같이 구현. but, useEffect는 매 렌더링마다 동기화를 하는 개념으로 이해하면 좋다.
            ```tsx
            useEffect(() => {
                // You can control new DOM elements
            }, [deps]);
            ```
    - Unmount
        - Component가 페이지 상에서 사라진 상태
        - Functional Component에서 아래와 같이 구현. but, useEffect는 매 렌더링마다 동기화를 하는 개념으로 이해하면 좋다.
            ```tsx
            useEffect(() => {
                return () => {
                    // You can clean up event listeners, clearTimeout, etc.
                };
            }, []);
            ```
        - clean up
            - clean up 과정: props나 state값이 변경 -> Component 재렌더링 -> 이전 side effects clean up -> 새로운 side effects 발생
            - clean up이 필요없는 effect: Network Request, DOM Control, Logging, etc.
            - clean up이 필요한 effect: Add Event Listener
                - clean up을 하지 않았을 때 발생 가능한 버그: 예를 들어 특정 페이지에서만 사용할 목적으로 window에 Event Listener 설정 후 clean up을 하지 않고 다른 페이지로 넘어가면, 해당 Event 발생 시 Event Handler 함수가 호출됨

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react)
## Hooks
1. useState
    - Functional Component에서 상태값을 다루기 위한 Hook
1. useEffect
    - Functional Component에서 DOM이 마운트되고 스크린에 그려진 후 비동기적으로 호출되는 Hook
    - Component Tree 외부에 있는 것들을 props나 state에 따라 동기화하는 것
        - Network Request, DOM Control, Add Event Listener, etc.
1. useLayoutEffect
    - Funcional Component에서 DOM이 마운트되고 스크린에 그 전에 동기적으로 호출되는 Hook
    - 동기적으로 호출되므로 많은 로직이 존재할 경우, 사용자가 레이아웃을 보기까지 시간이 오래 걸릴 수 있음
        ```tsx
        import { useEffect, useRef, useLayoutEffect } from 'react';

        const App = (): JSX.Element => {
            const inputRef = useRef<HTMLInputElement | null>(null);
            
            useEffect(() => {
                inputRef.current.value = 'another user';
            });

            useLayoutEffect(() => {
                console.log(inputRef.current.value);
            });
            
            return (
                <div>
                    <input type='text' value='EmmanuelTheCoder' ref={inputRef} />
                </div>
            );
        }
        
        export default App;
        
        // 화면에는 "another user"가 나오지만, 콘솔에는 "EmmanuelTheCoder"가 찍힌다.
        ```
1. useMemo
    - Functional Component에서 값을 메모이제이션하기 위해서 사용되는 Hook
    - useEffect가 a라는 변수에 따라 side effects가 발생해야 하는 상황에서 a 선언 시 b라는 변수의 값에 따라 값이 변하는 useMemo를 선언 후 사용하면, b라는 변수의 값이 변하지 않는 상황에서 Component가 리렌더링될 때 useEffect의 side effects가 발생하는 것을 방지 가능
        ```tsx
        import { useState, useMemo } from 'react';

        const CounterButton = ({ counter, setCounter }: { counter: number; setCounter: (counter: number) => void }): JSX.Element => {
            return (
                <div>
                    <button style={{
                        border: 'none',
                        borderRadius: '10px',
                        fontSize: '18px',
                        backgroundColor: 'skyblue',
                        cursor: 'pointer',
                    }}
                        onClick={() => {
                            setCounter(counter + 1);
                        }}>카운터 업데이트</button>
                </div>
            );
        };

        const OtherCounterButton = ({ otherCounter, setOtherCounter }: { otherCounter: number; setOtherCounter: (otherCounter: number) => void }): JSX.Element => {
            return (
                <div>
                    <button style={{
                        border: 'none',
                        borderRadius: '10px',
                        fontSize: '18px',
                        backgroundColor: 'red',
                        cursor: 'pointer',
                    }}
                        onClick={() => {
                            setOtherCounter(otherCounter + 1);
                        }}>다른 카운터 업데이트</button>
                </div>
            );
        };

        const App = (): JSX.Element => {
            const [counter, setCounter] = useState<number>(0);
            const [otherCounter, setOtherCounter] = useState<number>(0);
            const doubleNumber = useMemo(() => {
                return slowFunction(counter);
            }, [counter]);
            // const doubleNumber = slowFunction(counter);

            return (
                <div style={{
                    display: 'flex',
                    flexDirection: 'column',
                    justifyContent: 'center',
                    alignItems: 'center',
                    height: '100vh',
                }}>
                    <div style={{
                        display: 'flex',
                        flexDirection: 'row',
                        alignItems: 'center',
                    }}>
                        <p>카운터: {counter}</p>
                        <CounterButton counter={counter} setCounter={setCounter} />
                    </div>
                    <div style={{
                        display: 'flex',
                        flexDirection: 'row',
                        alignItems: 'center',
                    }}>
                        <p>다른 카운터: {otherCounter}</p>
                        <OtherCounterButton otherCounter={otherCounter} setOtherCounter={setOtherCounter} />
                    </div>
                    <p>카운터 X 2: {doubleNumber}</p>
                </div>
            );
        };

        const slowFunction = (num: number): number => {
            for (let i = 0; i < 500000000; i++) { }
            return num * 2;
        }

        export default App;
        ```
1. useCallback
    - Functional Component에서 함수를 메모이제이션하기 위해서 사용되는 Hook
    - useEffect가 a라는 함수에 따라 side effects가 발생해야 하는 상황에서 a 선언 시 b라는 변수의 값에 따라 내부 로직이 변하는 useCallback을 선언 후 사용하면, b라는 변수의 값이 변하지 않는 상황에서 Component가 리렌더링될 때 useEffect의 side effects가 발생하는 것을 방지 가능
1. useRef
    - Functional Component에서 Component Life-Cycle동안 사용 가능한 변수로서, DOM 요소 접근 또는 특정 값을 담기 위한 객체를 반환하는 Hook
        ```tsx
        import { useState, useRef } from 'react';

        const App = (): JSX.Element => {
            const refObject = useRef<number>(1);
            const rawObject = { current: 1 };

            const [isChanged, setIsChanged] = useState<boolean>(false);

            return (
                <>
                    <button onClick={() => {
                        refObject.current = 2;
                        rawObject.current = 2;
                    }}>값 변경</button>
                    <button onClick={() => {
                        setIsChanged(true);
                    }}>리렌더링</button>
                    <button onClick={() => {
                        console.log(refObject.current);
                        console.log(rawObject.current);
                    }}>값 확인</button>
                </>
            );
        };

        export default App;

        // 값 변경 버튼 클릭 -> 리렌더링 버튼 클릭 -> 값 확인 버튼 클릭 시,
        // refObject.current는 2 출력 (useRef가 반환한 객체는 Life-Cycle동안 값을 유지)
        // rawObject.current는 1 출력 (일반 JavaScript 객체는 리렌더링 시 값이 초기화됨)
        ```

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react)
## Performance Optimization
1. Input Element
    - 기존
        ```tsx
        import { useState, FormEvent, ChangeEvent } from 'react';

        const App = (): JSX.Element => {
            const [inputState, setInputState] = useState<string>('');

            return (
                <form onSubmit={(event: FormEvent<HTMLFormElement>) => {
                    event.preventDefault();

                    // do something with inputValue
                }}>
                    <input value={inputState} onChange={(event: ChangeEvent<HTMLInputElement>) => {
                        setInputState(event.target.value);
                    }} />
                </form>
            );
        };

        export default App;
        ```
    - 개선
        ```tsx
        import { useRef, FormEvent } from 'react';

        const App = (): JSX.Element => {
            const inputRef = useRef<HTMLInputElement | null>(null);

            return (
                <form onSubmit={(event: FormEvent<HTMLFormElement>) => {
                    event.preventDefault();

                    if (!inputRef.current) {
                        return;
                    }

                    // do something with inputRef.current.value
                }}>
                    <input ref={inputRef} />
                </form>
            );
        };

        export default App;
        ```
        
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#react)