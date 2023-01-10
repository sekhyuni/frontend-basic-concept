# Browser
## Basic Concept
1. 이벤트 버블링 & 이벤트 위임 & 이벤트 캡처링
    1. 이벤트 버블링
        - 특정 요소에서 이벤트가 발생했을 때 해당 이벤트가 특정 요소부터 최상위 요소인 html 요소까지 점점 더 상위 요소들로 전달되어가는 특성
        - 대표적으로 버블링되는 이벤트: click, mousedown, mouseup, wheel, scroll, keydown, keyup
            ```html
            <html>
                <head>
                    <style>
                        body {
                            background-color: gainsboro;
                            height: 500px;
                            width: 500px;
                        }

                        .pink-div {
                            position: relative;
                            background-color: pink;
                            height: 200px;
                            width: 200px;
                            display: flex;
                            justify-content: center;
                            align-items: center;
                        }

                        .green-div {
                            position: absolute;
                            top: 0;
                            left: 0;
                            background-color: aquamarine;
                            width: 100px;
                            height: 100px;
                        }
                    </style>
                </head>
                <body>
                    <div class='pink-div'>
                        <div class='green-div'></div>
                    </div>
                    <script>
                        const html = document.querySelector('html');
                        const body = document.querySelector('body');
                        const greenDiv = document.querySelector('.green-div');
                        const pinkDiv = document.querySelector('.pink-div');

                        html.addEventListener(
                            'click',
                            () => {
                                console.log('html');
                            }
                        );

                        body.addEventListener(
                            'click',
                            () => {
                                console.log('body');
                            }
                        );
                        pinkDiv.addEventListener(
                            'click',
                            () => {
                                console.log('pink div');
                            }
                        );

                        greenDiv.addEventListener(
                            'click',
                            () => {
                                console.log('green div');
                            }
                        );
                    </script>
                </body>
            </html>
            ```
    1. 이벤트 위임
        - 이벤트 버블링의 개념을 기반으로 하는 이벤트 처리 패턴
        - 구현 순서
            1. 컨테이너에 하나의 핸들러를 할당
            1. 핸들러의 event.target을 사용해서 이벤트가 발생한 요소가 어디인지 알아냄
            1. 원하는 요소에서 이벤트가 발생했다고 확인되면 이벤트를 핸들링
        - 장점
            1. 많은 핸들러를 할당하지 않아도 되기 때문에 초기화가 단순해지고 메모리가 절약됨
            1. 요소를 추가하거나 제거할 때 해당 요소에 할당된 핸들러를 추가하거나 제거할 필요가 없음
        - 단점
            1. 이벤트 위임을 사용하려면 이벤트가 반드시 버블링되어야 하는데, 몇몇 이벤트는 버블링 되지 않음
            1. 컨테이너에 할당된 핸들러가 모든 하위 요소에서 발생하는 이벤트에 응답해야 하므로 CPU 작업 부하가 늘어날 수 있으나, 이런 부하는 무시할만한 수준이므로 실제로는 잘 고려하지 않음
            ```html
                <div>
                    <section style="width: fit-content">
                        <button>Button 1</button>
                        <button>Button 2</button>
                        <button>Button 3</button>
                    </section>
                </div>
                <script>
                    const div = document.querySelector('div');

                    div.addEventListener('click', (event) => {
                        if (event.target.tagName === 'DIV') {
                            console.log('div');
                        }
                        if (event.target.tagName === 'SECTION') {
                            console.log('section');
                        }
                        if (event.target.tagName === 'BUTTON') {
                            console.log(event.target.innerText);
                        }
                    });
                </script>
            ```
    1. 이벤트 캡처링
        - 특정 요소에서 이벤트가 발생했을 때 해당 이벤트가 최상위 요소인 html 요소부터 특정 요소까지 점점 더 하위 요소들로 전달되어가는 특성
            ```html
            <html>
                <head>
                    <style>
                        body {
                            background-color: gainsboro;
                            height: 500px;
                            width: 500px;
                        }

                        .pink-div {
                            position: relative;
                            background-color: pink;
                            height: 200px;
                            width: 200px;
                            display: flex;
                            justify-content: center;
                            align-items: center;
                        }

                        .green-div {
                            position: absolute;
                            top: 0;
                            left: 0;
                            background-color: aquamarine;
                            width: 100px;
                            height: 100px;
                        }
                    </style>
                </head>
                <body>
                    <div class='pink-div'>
                        <div class='green-div'></div>
                    </div>
                    <script>
                        const html = document.querySelector('html');
                        const body = document.querySelector('body');
                        const greenDiv = document.querySelector('.green-div');
                        const pinkDiv = document.querySelector('.pink-div');

                        html.addEventListener(
                            'click',
                            () => {
                                console.log('html');
                            },
                            true
                        );

                        body.addEventListener(
                            'click',
                            () => {
                                console.log('body');
                            },
                            true
                        );
                        pinkDiv.addEventListener(
                            'click',
                            () => {
                                console.log('pink div');
                            },
                            true
                        );

                        greenDiv.addEventListener(
                            'click',
                            () => {
                                console.log('green div');
                            },
                            true
                        );
                    </script>
                </body>
            </html>
            ```
    1. 이벤트 전파 막기
        ```javascript
        event.stopPropagation(); // 버블링 또는 캡처링 전파를 막고 싶을 때
        event.stopImmediatePropagation(); // 버블링 또는 캡처링 전파뿐만 아니라, 현재 실행중인 이벤트 핸들러 이후 어떤 이벤트 핸들러도 실행시키지 않고 싶을 때
        ```
