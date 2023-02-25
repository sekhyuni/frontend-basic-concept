# CSS

* [display](#display)
* [position](#position)
* [Layout Shift](#layout-shift)
* [ETC](#etc)

## display
- block
    - 한 줄을 점유하고 있으며, 다음 요소는 줄바꿈 발생
    - width/height/padding/margin 스타일 적용 가능
    - 대표적인 요소: div, header, main, footer, section, form, ul, li, table, p, etc.
    - block에서의 width: auto와 height: auto
        1. width: auto (기본값)
            - 부모 요소를 기준으로 함 (부모 width와 동일)
                ```css
                .parent {
                    width: 200px;
                }
                
                <!-- .child는 width가 200px로 자동 설정 -->
                ```
        1. height: auto (기본값)
            - 자식 요소를 기준으로 함 (자식 width + padding + border와 동일)
                ```css
                .child {
                    height: 200px;
                    border: 1px;
                    padding: 10px;
                }
                
                <!-- .parent는 height가 222px로 자동 설정 -->
                ```
- inline
    - text 크기만큼 공간을 점유하고 있으며, 줄바꿈 발생하지 않음
    - width/height/padding/margin 스타일 적용 불가
    - 대표적인 요소: button, a, input, span, img, label, etc.
- inline-block
    - text 크기만큼 공간을 점유하고 있으며, 줄바꿈 발생하지 않음
    - width/height/padding/margin 스타일 적용 가능
- flex
    - 속성
        ```css
        display: flex;
        flex-direction: row | column;
        justify-content: start | center | end;
        align-items: start | center | end;
        flex-wrap: no-wrap | wrap;
        ```
    - flex-direction이 row인 경우 기본적으로 자식 요소의 width는 content만큼 적용되며, height는 상속을 받음
    - flex-direction이 column인 경우 기본적으로 자식 요소의 width는 상속을 받으며, height는 content만큼 적용됨
    - 자식 요소 css에 flex-grow: 1;을 적용하면, calc(100% - {anotherElementSize})와 같은 효과를 낼 수 있음
            
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#css)
## position
- 속성
    ```css
    position: static | relative | absolute | fixed | sticky;
    ```
- 값
    - static: 기본값이며, 위치를 임의로 지정 불가
    - relative: 원래 있던 위치를 기준으로 좌표 지정
    - absolute: 절대 좌표와 함께 위치 지정
        - top, left, bottom, right 속성과 같이 쓰임
        - 상위 레벨 Element 중 속성값이 relative 또는 absolute인 것이 있으면 해당 Element 기준 위치 지정
        - 상위 레벨 Element 중 속성값이 relative 또는 absolute인 것이 없으면 최상위 Element 기준 위치 지정 
    - fixed: 스크롤과 상관없이 항상 문서 최좌측상단을 기준으로 좌표 고정
    - sticky: 평소에는 문서 내에서 static 속성값과 같이 일반적인 흐름을 따르지만 스크롤 위치가 임계치에 이르면 fixed 속성값과 같이 좌표 고정

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#css)
## Layout Shift
- 레이아웃 이동이란 화면상의 요소 변화로 레이아웃이 갑자기 밀리는 현상
- CLS가 발생하는 이유
    1. 크기가 정해지지 않은 이미지
    1. 크기가 정해지지 않은 광고, 임베드 및 iframe
    1. 동적으로 주입된 콘텐츠
    1. FOIT/FOUT을 유발하는 웹 글꼴
    1. DOM을 업데이트하기 전에 네트워크 응답을 대기하는 작업
- CLS를 측정하는 방법
    1. Lighthouse를 통해 Analyze page load 클릭 후, View Original Trace를 클릭하면 Chrome DevTools Performance 탭으로 이동되어 Layout Shift가 발생한 요소를 확인할 수 있음
- CLS를 개선하는 방법
    1. 이미지 및 비디오 요소에 항상 크기 속성을 포함하거나 CSS 가로 세로 비율 상자와 같은 방식으로 필요한 공간을 미리 확보
    1. 사용자 상호 작용에 대한 응답을 제외하고는 기존 콘텐츠 위에 콘텐츠를 삽입하지 않기
    1. 레이아웃 변경을 트리거하는 속성의 애니메이션보다 전환 애니메이션을 사용하기

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#css)
## ETC
1. CSS-in-JS
    - 각 컴포넌트 별 고유 네임 스페이스에 CSS 작성 가능
        - 복잡한 애플리케이션 개발 시, 선택자 충돌 방지 가능 ({file}.module.css 형식으로 사용하면 CSS-in-CSS로도 선택자 충돌 방지 가능)
    - JavaScript와 CSS간에 상수와 함수 공유 가능 (props를 사용하여 조건부 스타일링 가능)

[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#css)