# Browser
## Basic Concept
1. Event Bubbling & Capturing
    1. Event Bubbling
        - 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어가는 특성
        - 특정 요소에 이벤트가 발생하면 해당 요소에 할당된 핸들러가 동작하고 이어서 부모 요소의 핸들러가 동작
            ```html
                <!-- [예시 1] -->
                <div class='parent'>
                    부모 요소
                    <div class='child'>
                        자식 요소
                        <div class='grandchild'>
                            손자 요소
                        </div>
                    </div>
                </div>
                <script>
                    document.querySelectorAll('div').forEach(element => {
                        element.addEventListener('click', function (event) {
                            console.log(event.currentTarget.className);
                        })
                    });
                </script>

                <!-- 손자 요소 클릭 시, grandchild -> child -> parent 순으로 event handler 동작 -->
                <!-- 자식 요소 클릭 시, child -> parent 순으로 event handler 동작 -->
                <!-- 부모 요소 클릭 시, parent event handler 동작 -->
            ```
            ```html
                <!-- 예시 2 -->
                <div class='parent'>
                    부모 요소
                    <div class='child'>
                        자식 요소
                    </div>
                </div>
                <script>
                    document.querySelector('.parent').addEventListener('mousedown', function (event) {
                        console.log(event.currentTarget.className);
                    });

                    document.querySelector('.child').addEventListener('click', function () {
                        console.log(event.currentTarget.className)
                    });
                </script>

                <!-- 자식 요소 클릭 시, parent -> child 순으로 event handler 동작 -->
                <!-- 부모 요소 클릭 시, parent event handler 동작 -->
            ```
    1. Event Capturing
