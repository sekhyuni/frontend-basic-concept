# JavaScript
## Basic Concept
1. 실행 컨텍스트
    - 구성 : Variable Object, Scope Chain, this
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
1. 비동기 처리
    - 기본적으로 JavaScript Engine은 Call Stack이 1개이므로 JavaScript 런타임상에서 모든 Task가 동기적으로 수행되어야 할 것 같지만, Browser 또는 Node.js 환경 내부에 존재하는 Web API, Event Queue, Event Loop를 통해서 비동기 처리가 가능
1. Prototype
    - 부모 역할을 담당하는 객체
    - Prototype Chain : 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없다면 자신의 부모 역할을 하는 Prototype 객체의 프로퍼티나 메소드를 차례대로 검색 
1. Closure
    - 외부함수의 실행 컨텍스트가 소멸되어도 외부함수의 Scope에 접근할 수 있는 내부함수