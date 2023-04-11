# Browser

* [Rendering Process](#rendering-process)
* [Communication Process](#communication-process)
* [CSR vs SSR](#csr-vs-ssr)
* [Local Storage vs Session Storage vs Cookie](#local-storage-vs-session-storage-vs-cookie)
* [CORS](#cors)
* [Cache](#cache)
* [Event Bubbling vs Event Capturing](#event-bubbling-vs-event-capturing)

## Rendering Process
1. HTML을 Parsing해서 **DOM Tree**를 생성하고, HTML에 CSS가 포함되어 있다면 **CSSOM Tree**도 함께 생성
1. DOM Tree와 CSSOM Tree를 합쳐서 **Render Tree**를 생성
1. Render Tree를 통해 **Layouting**
1. **Painting**

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)
## Communication Process
- 먼저 www.naver.com의 IP를 찾기 위해 DNS 서버에서 해당 도메인 네임에 매핑된 IP를 얻어오고, 그 다음 해당 IP의 서버와 TCP 연결을 맺게 됩니다. 이후 HTTP 요청이 진행되고, 그에 맞는 HTTP 응답을 받아서 브라우저에 전달 후, 응답받은 페이지가 브라우저에 렌더링되는 순서로 진행되게 됩니다.

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)
## CSR vs SSR
||Where|Initial Loading Speed|Page Reload|SEO Friendly|
|:---:|:---:|:---:|:---:|:---:|
|CSR|Browser|Slower than SSR|X|X|
|SSR|Web Server (Node.js, Tomcat, etc.)|Fast than CSR|O|O|
- Next.js에서의 Rendering은 CSR과 SSR이 혼합된 방식임
    - CSR
        - next/link의 Link 컴포넌트가 클릭됐을 때
        - next/router의 router.push 함수가 호출됐을 때
    - SSR
        - 초기 페이지가 로드됐을 때
        - 페이지가 리로드됐을 때
        - anchor 요소가 클릭됐을 때

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)
## Local Storage vs Session Storage vs Cookie
||Capacity|Storage Space|Expiration|Automatically sent on http request|Accessible|Value Type|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Local Storage|10MB|Both disk and browser memory|Until manually cleared or deleted|X|All domains and subdomains|string|
|Session Storage|5MB|Only browser memory|Until browser tab is closed|X|All domains and subdomains|string|
|Cookie|4KB|Only browser memory|Until the set expiration time is over|O|All domains and subdomains|string|

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)
## CORS
- 기본적으로 브라우저에서 서버의 응답을 받기 위해서는 Same Origin(Protocol, IP, Port)일 경우에만 가능했으나, Cross Origin Resource Sharing 정책이 등장하면서 Cross Origin인 경우에도 상호작용이 가능해졌음. CORS를 정석적으로 사용하기 위해서는 서버 HTTP 응답 헤더의 Access-Control-Allow-Origin에 클라이언트 Origin 정보를 추가해주면 되며, webpack-dev-server에 Proxy 설정을 함으로써 서버측의 작업없이도 CORS 이슈를 우회하여 해결할 수 있음
- Preflight Request
    - 본격적인 Cross Origin HTTP 요청 전에 **서버 측에서 그 요청의 메서드와 헤더에 대해 인식하고 있는지 체크**하는 것
    - 일반적으로 **브라우저에서 자동적으로 발생**하며, **OPTION 메서드**를 통해 HTTP 요청 헤더에 **Access-Control-Allow-Header, Access-Control-Allow-Methods, Origin** 정보를 담아서 요청
    - 사전 요청을 보냄으로써 **CORS를 인식할 수 없는 서버를 악성 요청으로부터 보호**할 수 있음
- Frontend 서버에 Proxy 설정 시, CORS 이슈 우회 동작 원리
    1. 브라우저에서 API 요청 시, Proxy 서버로 요청을 날림 **(클라이언트와 Proxy 서버는 Same Origin이므로 별도 설정없이 상호작용이 가능)**
        - Proxy 서버는 로컬에서 실행되는 서버이며, Frontend App이 실행되는 서버와 Proxy 서버가 각각 실행되는 것이 아니라, Frontend App이 실행되는 서버가 Proxy 역할을 수행함. 따라서 Frontend App이 localhost:3000이라는 Host로 실행되면, Proxy 서버의 Host도 localhost:3000가 됨
        - 만약 Proxy 서버가 Frontend App이 실행되는 서버와 다른 Host로 실행된다고 해도 Proxy 서버에서 HTTP 응답 헤더의 Access-Control-Allow-Origin에 클라이언트 Origin 정보를 추가해주면 해결됨
    1. Proxy 서버로 요청이 들어오면 해당 요청을 API 서버로 보냄
        - **Proxy 서버에서 해당 요청을 API 서버로 보낼 수 있는 이유는 브라우저를 통하지 않는 상호작용이기 때문임**
    1. API 서버에서 요청 처리 후, 응답을 Proxy 서버로 보냄
    1. Proxy 서버는 전달 받은 **응답 헤더의 Access-Control-Allow-Origin에 클라이언트 Origin을 추가해서 브라우저로 보냄**
    1. 브라우저는 응답 헤더를 확인하여 CORS 설정이 잘 되어있다는 판단 하에 정상 처리함
- 클라이언트에서 자체적으로 CORS 이슈 우회하기
    - Proxy 설정 시, CORS 이슈를 우회할 API 서버의 Origin을 설정해야 함
    - axios 사용 시, baseURL에 API 서버의 Origin을 설정하지 않아야 함
        1. React (setupProxy.js에서 http-proxy-middleware 라이브러리 사용) -> 개발 모드만 가능
            ```javascript
            // setupProxy.js
            const { createProxyMiddleware } = require('http-proxy-middleware');

            const proxy = {
                target: 'http://{apiServerIP}:{apiServerPort}',
            }

            module.exports = app => {
                app.use([
                    '/a',
                    '/b',
                    '/c',
                ],
                    createProxyMiddleware(proxy)
                );
            };
            ```
            ```typescript
            // index.ts
            import Axios from 'axios';

            const isProd = process.env.NODE_ENV === 'production';

            export const apiServerOrigin = 'http://{apiServerIP}:{apiServerPort}';

            const axios = Axios.create({
                baseURL: isProd ? apiServerOrigin : '',
            });

            export default axios;
            ```
        1. Next.js (next.config.js에서 rewrite 함수 설정 사용) -> 개발, 운영 모드 둘다 가능
            ```javascript
            // next.config.js
            /** @type {import('next').NextConfig} */
            const nextConfig = {
                reactStrictMode: true,
                async rewrites() {
                    return [
                        {
                            source: '/:path*',
                            destination: 'http://{apiServerIP}:{apiServerPort}/:path*',
                        },
                    ];
                },
            };

            module.exports = nextConfig;
            ```
            ```typescript
            // index.ts
            import Axios from 'axios';

            const isProd = process.env.NODE_ENV === 'production';

            export const apiServerOrigin = 'http://{apiServerIP}:{apiServerPort}';

            const axios = Axios.create({
                baseURL: isProd ? apiServerOrigin : '',
            });

            export default axios;
            ```
            
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)
## Cache
- Update later..

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)
## Event Bubbling vs Event Capturing
1. 이벤트 버블링
    - 특정 요소에서 이벤트가 발생했을 때 해당 이벤트가 **가장 하위에 있는 요소부터 최상위 요소인 html 요소까지** 점점 더 상위 요소들로 전달되어가는 특성
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
    - 이벤트 버블링을 활용한 예
        - Event Delegation
            - 이벤트 버블링의 개념을 기반으로 하는 이벤트 처리 패턴
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
1. 이벤트 캡처링
    - 특정 요소에서 이벤트가 발생했을 때 해당 이벤트가 **최상위 요소인 html 요소부터 가장 하위에 있는 요소까지** 점점 더 하위 요소들로 전달되어가는 특성
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
    
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#browser)