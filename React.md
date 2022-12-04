# React
## Basic Concept
1. Life-Cycle
    - 순서: Mount -> Update -> Unmount
    - 상태
        - Mount
            - Component가 렌더링되어 DOM을 조작할 수 있는 상태
            - Functional Component에서 아래와 같이 구현. but, useEffect는 매 렌더링마다 동기화를 하는 것
                ```javascript
                    useEffect(() => {
                        // You can control DOM elements
                    }, []);
                ```
        - Update
            - Component 내 props나 state값이 변경되면서 Component가 재렌더링되어 변경된 DOM을 조작할 수 있는 상태
            - Functional Component에서 아래와 같이 구현. but, useEffect는 매 렌더링마다 동기화를 하는 개념
                ```javascript
                    useEffect(() => {
                        // You can control new DOM elements
                    }, [deps]);
                ```
        - Unmount
            - Component가 페이지 상에서 사라진 상태
            - Functional Component에서 아래와 같이 구현. but, useEffect는 매 렌더링마다 동기화를 하는 개념
                ```javascript
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
        ```javascript
            import React, { useRef, useEffect, useLayoutEffect } from 'react';

            const App = () => {
              const inputRef = useRef<HTMLInputElement | null>(null);
              
              useEffect(()=>{
                inputRef.current.value = 'another user';
              });

              useLayoutEffect(()=>{
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
1. useCallback
    - Functional Component에서 함수를 메모이제이션하기 위해서 사용되는 Hook
    - useEffect의 deps에 특정 함수를 넣어서 사용할 때, 해당 함수 선언 시 useCallback을 특정 변수를 갖는 deps와 함께 사용하면 Component가 렌더링될 때마다 useEffect 내의 side effects가 발생하는 것을 방지 가능
1. useRef
    - Functional Component에서 Component Life-Cycle동안 사용 가능한 변수로서, DOM 요소 접근 또는 특정 값을 담기 위한 객체를 반환하는 Hook
        ```javascript
            const App = () => {
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

            // 값 변경 버튼 클릭 -> 리렌더링 버튼 클릭 -> 값 확인 버튼 클릭 시,
            // refObject.current는 2 출력 (useRef가 반환한 객체는 Life-Cycle동안 값을 유지)
            // rawObject.current는 1 출력 (일반 JavaScript 객체는 리렌더링 시 값이 초기화됨)
        ```

## Performance
1. Input Element Optimization
    - 기존
        ```javascript
            const [inputValue, setInputValue] = useState<string>('');

            return (
                <form onSubmit={(event: FormEvent<HTMLFormElement>) => {
                    event.preventDefault();

                    // do something with inputValue
                }}>
                    <input value={inputValue} onChange={(event: ChangeEvent<HTMLInputElement>) => {
                        setInputValue(event.target.value);
                    }} />
                </form>
            );
        ```
    - 개선
        ```javascript
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
        ```
