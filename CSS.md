# CSS

* [display](#display)
* [position](#position)
* [flex](#flex)
* [ETC](#etc)

## display
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
                heigth: 200px;
                border: 1px;
                padding: 10px;
            }
            
            <!-- .parent는 height가 222px로 자동 설정 -->
            ```
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
## flex
- 속성
    ```css
    display: flex;
    flex-direction: row | column;
    justfiy-content: start | center | end;
    align-items: start | center | end;
    flex-wrap: no-wrap | wrap;
    ```
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#css)
## ETC
1. CSS-in-JS
    - 각 컴포넌트 별 고유 네임 스페이스에 CSS 작성 가능
        - 복잡한 애플리케이션 개발 시, 선택자 충돌 방지 가능 ({file}.module.css 형식으로 사용하면 CSS-in-CSS로도 선택자 충돌 방지 가능)
    - JavaScript와 CSS간에 상수와 함수 공유 가능 (props를 사용하여 조건부 스타일링 가능)
[메인으로 가기](https://github.com/sekhyuni/frontend-basic-concept)</br>
[맨 위로 가기](#css)