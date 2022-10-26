# React
1. Life-Cycle
    - 순서 : Mount -> Update -> Unmount
    - Mount
        - 컴포넌트가 렌더링되어 DOM을 조작할 수 있는 상태
            ```javascript
            useEffect(() => {
                // You can control DOM elements
            }, []);
            ```
    - Update
        - 컴포넌트 내 props나 state값이 변경되어 컴포넌트가 재렌더링되어
            ```javascript
            useEffect(() => {
                // You can control DOM elements
            }, [deps]);
            ```
    - Unmount
        - ㅇㅇㅇ
            ```javascript
            useEffect(() => {
                // You can control DOM elements
            }, [deps]);
            ```
1. useState
    - Functional Component에서 상태값을 다루기 위한 Hook
1. useEffect
    - Functional Component에서 React의 Life-Cycle을 다루기 위한 Hook