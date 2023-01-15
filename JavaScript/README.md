# JavaScript

* [Execution Context](#execution-context)
* [Hoisting](#hoisting)
* [Asynchronous Processing](#asynchronous-processing)
* [Prototype](#prototype)
* [Closure](#closure)
* [Object vs Map](#object-vs-map)

## Execution Context
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

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#javascript)
## Hoisting
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

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#javascript)
## Asynchronous Processing
- 기본적으로 JavaScript Engine은 Call Stack이 1개이므로 JavaScript 런타임상에서 모든 Task가 동기적으로 수행되어야 할 것 같지만, Browser 또는 Node.js 환경 내부에 존재하는 Web API, Event Queue, Event Loop를 통해서 비동기 처리가 가능
    - Web API: setTimeout, setInterval, XMLHttpRequest, Promise, requestAnimationFrame 등의 실질적인 비동기 이벤트 처리 및 비동기 네트워크 통신 처리를 담당
    - Event Queue: Task Queue, Microtask Queue, Animation Frames와 같이 Task가 Call Stack으로 옮겨지기 전에 대기하는 Queue를 일컬음
        - Call Stack으로 옮겨지는 순서: Microtask Queue -> Animation Frames -> Task Queue
    - Event Loop: 지속적으로 Call Stack과 Event Queue를 확인하여, Call Stack이 비워져 있는 경우 Event Queue에서 Task를 꺼내어 Call Stack으로 옮김

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#javascript)
## Prototype
- 부모 역할을 담당하는 객체
- Prototype Chain: 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없다면 자신의 부모 역할을 하는 Prototype 객체의 프로퍼티나 메소드를 차례대로 검색 

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#javascript)
## Closure
- 외부함수의 실행 컨텍스트가 소멸되어도 외부함수의 Scope에 접근할 수 있는 내부함수
- 클로저를 활용한 예
    - Debouncing vs Throttling
        1. 디바운씽
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
        1. 쓰로틀링
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

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#javascript)
## Object vs Map
1. Object
    - Object는 iterable하지 않음
    - Object의 key는 String 또는 Symbol 타입으로만 설정 가능
    - 빈번한 추가 및 제거 작업에서 Map보다 성능이 낮음
    - Serialization 또는 Parsing을 기본적으로 지원함
    - V8 Engine에서 Object는 Hash Table로 구현되어 있지 않은 것으로 알려짐
        ```javascript
            var obj = {};
            var obj = Object.create(null);
            obj.key = 1;
            obj.key += 10;
            for (const k in obj) obj[k]++;
            let sum = 0;
            for (const v of Object.values(obj)) sum += v;
            if ('key' in obj);
            if (obj.hasOwnProperty('key'));
            delete (obj.key);
            Object.keys(obj).length;
        ```
1. Map
    - Map은 iterable함
    - Map의 key는 모든 타입으로 설정 가능
    - 빈번한 추가 및 제거 작업에서 Object보다 성능이 높음
    - Serialization 또는 Parsing을 기본적으로 지원하지 않음
    - V8 Engine에서 Map은 Hash Table로 구현되어 있음
        ```javascript
            const hashTable = new Map();
            hashTable.set('key', 1);
            hashTable.set('key', hashTable.get('key') + 10);
            hashTable.foreach((k, v) => hashTable.set(k, hashTable.get(k) + 1));
            for (const k of hashTable.keys()) hashTable.set(k, hashTable.get(k) + 1);
            let sum = 0;
            for (const v of hashTable.values()) sum += v;
            if (hashTable.has('key'));
            hashTable.delete('key');
            hashTable.size();
        ```
        
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#javascript)