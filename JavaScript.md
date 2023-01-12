# JavaScript
## Basic Concept
1. Execution Context
    - 구성: Variable Object, Scope Chain, this
        - this
            - this에 할당되는 값은 함수 호출 패턴에 의해 결정
            - this value가 결정되기 이전에 this는 전역 객체를 가리키고 있다가 함수 호출 패턴에 의해 this에 할당되는 값이 결정
                ```javascript
                function Test() {
                    setTimeout(function () { // Normal Function에서 this는 호출 패턴에 의해 결정
                        console.log('In Normal Function');
                        console.log(this);
                    });
                    setTimeout(() => { // Arrow Function에서 this는 Lexical Scope를 따름
                        console.log('In Arrow Function');
                        console.log(this);
                    });
                }

                new Test();

                // In Normal Function에서는 Timeout 객체 출력
                // In Arrow Function에서는 Test 객체 출력
                ```
1. Hoisting
    - 인터프리터가 코드를 실행하기 전에 함수, 변수 또는 클래스의 선언을 해당 Scope의 맨 위로 이동하는 것처럼 보이는 프로세스
    - 변수 선언 키워드별 동작
        - var: 선언+초기화, 할당이 각각 따로 실행
            ```javascript
            console.log(a);
            var a = 1;

            // undefined 출력
            ```
        - let: 선언, 초기화, 할당이 각각 따로 실행 (Temporal Dead Zone 존재)
            ```javascript
            console.log(a);
            let a = 1;

            // ReferenceError: Cannot access 'a' before initialization 출력
            ```
        - const: 선언, 초기화+할당이 각각 따로 실행 (Temporal Dead Zone 존재)
            ```javascript
            console.log(a);
            const a = 1;

            // ReferenceError: Cannot access 'a' before initialization 출력
            ```
1. 비동기 처리
    - 기본적으로 JavaScript Engine은 Call Stack이 1개이므로 JavaScript 런타임상에서 모든 Task가 동기적으로 수행되어야 할 것 같지만, Browser 또는 Node.js 환경 내부에 존재하는 Web API, Event Queue, Event Loop를 통해서 비동기 처리가 가능
        - Web API: setTimeout, setInterval, XMLHttpRequest, Promise, requestAnimationFrame 등의 실질적인 비동기 이벤트 처리 및 비동기 네트워크 통신 처리를 담당
        - Event Queue: Task Queue, Microtask Queue, Animation Frames와 같이 Task가 Call Stack으로 옮겨지기 전에 대기하는 Queue를 일컬음
            - Call Stack으로 옮겨지는 순서: Microtask Queue -> Animation Frames -> Task Queue
        - Event Loop: 지속적으로 Call Stack과 Event Queue를 확인하여, Call Stack이 비워져 있는 경우 Event Queue에서 Task를 꺼내어 Call Stack으로 옮김
1. Prototype
    - 부모 역할을 담당하는 객체
    - Prototype Chain: 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없다면 자신의 부모 역할을 하는 Prototype 객체의 프로퍼티나 메소드를 차례대로 검색 
1. Closure
    - 외부함수의 실행 컨텍스트가 소멸되어도 외부함수의 Scope에 접근할 수 있는 내부함수
1. Debouncing vs Throttling
    1. Debouncing
        - 동일 이벤트가 반복적으로 일어나는 경우 마지막 이벤트가 일어나고 나서 일정 시간(ms)동안 해당 이벤트가 다시 일어나지 않으면 해당 이벤트의 콜백함수를 실행시키는 기술
            ```html
            <input placeholder='search' />
            <script>
                const debounce = (callback, delay) => {
                    let timer;
                    return (...args) => {
                        clearTimeout(timer);
                        timer = setTimeout(() => {
                            callback(...args);
                        }, delay);
                    };
                };

                const getSearchResults = (keyword) => {
                    return new Promise((resolve, reject) => {
                        setTimeout(() => {
                            resolve(`Results of ${keyword} are ..`);
                        }, 50);
                    });
                };

                const searchHandler = async (event) => {
                    const { value } = event.target;
                    const results = await getSearchResults(value);
                    console.log(results);
                };

                const optimisedSearchHandler = debounce(searchHandler, 500);

                const input = document.querySelector('input');
                
                input.addEventListener('keyup', (event) => {
                    // searchHandler(event);
                    optimisedSearchHandler(event);
                });
            </script>
            ```
    1. Throttling
        - 동일 이벤트가 반복적으로 일어나는 경우 이벤트의 실제 반복 주기와 상관없이 임의로 설정한 일정 시간(ms) 간격으로 콜백함수를 실행시키는 기술
            ```html
            <script>
                const throttle = (callback, interval) => {
                    let shouldWait = false;
                    return (...args) => {
                        if (shouldWait) {
                            return;
                        }

                        callback();
                        shouldWait = true;
                        setTimeout(() => {
                            shouldWait = false;
                        }, interval);
                    };
                };

                const handlerTrigger = () => {
                    console.log('Fire shot');
                };

                const optimisedTriggerHandler = throttle(handlerTrigger, 500);

                window.addEventListener('mousemove', () => {
                    // handlerTrigger();
                    optimisedTriggerHandler();
                });
            </script>
            ```
1. Object vs Map
    1. Object
        - Object does not implement an iteration protocol, and so objects are not directly iterable using the JavaScript for...of statement (by default)
        - The keys of an Object must be either a String or a Symbol
        - Not optimized for frequent additions and removals of key-value pairs
        - Native support for serialization or parsing.
            ```javascript
                var o = {};
                var o = Object.create(null);
                o.key = 1;
                o.key += 10;
                for(let k in o) o[k]++;
                var sum = 0;
                for(let v of Object.values(m)) sum += v;
                if('key' in o);
                if(o.hasOwnProperty('key'));
                delete(o.key);
                Object.keys(o).length;
            ```
    1. Map
        - A Map is an iterable, so it can be directly iterated
        - A Map's keys can be any value (including functions, objects, or any primitive)
        - Performs better in scenarios involving frequent additions and removals of key-value pairs
        - No native support for serialization or parsing
            ```javascript
                var m = new Map();
                m.set('key', 1);
                m.set('key', m.get('key') + 10);
                m.foreach((k, v) => m.set(k, m.get(k) + 1));
                for(let k of m.keys()) m.set(k, m.get(k) + 1);
                var sum = 0;
                for(let v of m.values()) sum += v;
                if(m.has('key'));
                m.delete('key');
                m.size();
            ```
