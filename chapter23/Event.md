# 이벤트


## 이벤트 핸들러
이벤트가 발생했을 떄 브라우저에 호출을 위임한 함수. (브라우저에 의해 호출될 함수)

### 이벤트 핸들러 등록 방법

#### 어트리뷰트 방식

어트리뷰트의 이름은 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다. 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문을 할당하면 이벤트 핸들러가 등록됨

```html
<!DOCTYPE HTML>
<html>
  <body>
    <button onclick= "sayHi()">Click me!</button>
    <script>
        function sayHi() {
        console.log('hi')
        }
    </script>
   </body>
</html>
```

CBD(Component-Based-Development) 방식의 Angular/React/Svelte/Vue.js 같은 프레임워크/라이브러리에서는 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 처리한다.


#### 프로퍼티 방식

HTML과 자바스크립트가 뒤섞이는 문제를 해결할 수 있지만, 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있음.


```javascript
// button은 이벤트 타깃
// onclick은 on + 이벤트 타입
// function은 이벤트 핸들러
button.onclick = function() {
    console.log('click');
}
```

```html
<!DOCTYPE HTML>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const button = document.querySelector('button');

      button.onclick = function() {
        console.log('click!');
      }
    </script>
   </body>
</html>
```

#### addEventListener 메서드 방식

어트리뷰트, 프로퍼티 방식과는 달리 이벤트 타입에 on 접두사를 붙이지 않음.

```javascript
EventTarget.addEventListener('eventType', functionName [, useCapture]);
// useCapture의 경우 capture 사용 여부로 true 값을 주면 capturing, 생략 또는 false 값을 주면 bubbling(기본값) 
```

```html
<!DOCTYPE HTML>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const button = document.querySelector('button');
      
      button.addEventListener('click', () => {
        console.log('click');
      })

      button.addEventListener('click', () => {
        console.log('click 2')
      })
    </script>
   </body>
</html>
```
`addEventListener` 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않기 때문에 이벤트 핸들러가 모두 호출된다.

#### 이벤트 핸들러 제거

- `removeEventListener` : `addEventListener`에 전달한 인수와 일치해야 제거된다. (익명함수를 이벤트 핸들러로 등록한 경우 제거❌)

```html
<!DOCTYPE HTML>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const button = document.querySelector('button');
      
      function handleClick() {
        console.log('click');
      }
      
      button.addEventListener('click', handleClick);
      
      button.removeEventListener('click', handleClick);
    </script>
   </body>
</html>
```

- 프로퍼티 방식의 경우 : 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 `null` 값을 할당하면 된다.

```html
<body>
    <button>Click me!</button>
    <script>
      const button = document.querySelector('button');
      
      button.onclick = function() {
        console.log('click me');
      }
      
      button.onclick = null;
    </script>
   </body>
```

## 이벤트 객체

이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성됨. 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달됨.

```javascript
const button = document.querySelector('button');

// event 객체...
function handleClick(event) {
  console.log(event.target)
}

button.addEventListener('click', handleClick)
```

## 이벤트 전파

이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다.


```plainText
캡처링 단계 : 이벤트가 상위 요소에서 하위 요소 방향으로 전파
타깃 단계 : 이벤트가 이벤트 타깃에 도달
버블링 단계 : 이벤트가 하위 요소에서 상위 요소 방향으로 전파
```

이처럼 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있다.
대부분의 이벤트는 캡처링과 버블링을 통해 전파된다.

```html
<ul id="fruits">
  <li id="apple">apple</li>
  <li id="banana">banana</li>
  <li id="kiwi">kiwi</li>
</ul>
````
이벤트 타깃이 li라고 할 경우  
캡처링 단계는 `window → document → html → body → ul → li`  
버블링 단계는 `li → ul → body → html → document → window`  
이런 식으로 전파된다.


```plainText
포커스 이벤트 : focus / blur
리소스 이벤트 : load / unload / abort / error
마우스 이벤트 : mouseenter / mouseleave
```
위 이벤트는 `event.bubbles`의 값이 false로 버블링을 통해 전파되지 않는다.


버블링 되지 않는 이벤트 타깃의 상위 요소에서 이벤트를 캐치하려면 캡쳐링 단계의 이벤트를 캐치해야 되는데, 대체할 수 있는 이벤트들이 있다.

```plainText
대체할 수 있는 이벤트
포커스 이벤트 : focusin / focusout
마우스 이벤트 : mouseover / mouseout
```


## 이벤트 위임

- 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다.
- 이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.
- 이벤트 위임을 통해 상위 DOM 요소에 이벤트를 바인딩한 경우 `event.target, event.currentTarget`이 다른 DOM 요소를 가리킬 수 있다.

