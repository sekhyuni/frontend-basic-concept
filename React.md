# React
## Basic Concept
1. Life-Cycle
    - 순서 : Mount -> Update -> Unmount
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
                    // You can clean up event listeners, clearTimeout, etc.
                    return () => {
                    };
                }, []);
                ```
            - clean up
                - clean up 과정 : props나 state값이 변경 -> Component 재렌더링 -> 이전 side effects clean up -> 새로운 side effects 발생
                - clean up이 필요없는 effect : Network Request, DOM Control, Logging, etc.
                - clean up이 필요한 effect : Add Event Listener
                    - clean up을 하지 않았을 때 발생 가능한 버그 : 예를 들어 특정 페이지에서만 사용할 목적으로 window에 Event Listener 설정 후 clean up을 하지 않고 다른 페이지로 넘어가면, 해당 Event 발생 시 Event Handler 함수가 호출됨

## Hooks
1. useState
    - Functional Component에서 상태값을 다루기 위한 Hook
1. useEffect
    - Functional Component에서 Component가 렌더링될 때마다 호출되는 Hook
    - Component Tree 외부에 있는 것들을 props나 state에 따라 동기화하는 것
        - Network Request, DOM Control, Add Event Listener, etc.
1. useLayoutEffect
1. useMemo
1. useCallback
    - Functional Component에서 함수를 메모이제이션하기 위해서 사용되는 Hook
    - useEffect의 deps에 특정 함수를 넣어서 사용할 때, 해당 함수 선언 시 useCallback을 특정 변수를 갖는 deps와 함께 사용하면 Component가 렌더링될 때마다 useEffect 내의 side effects가 발생하는 것을 방지 가능
1. useRef
