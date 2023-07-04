# Promise

비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다. 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또 비동기 함수를 호출해야 한다. 이처럼 콜백 함수 호출의 중첩으로 인해 복잡도가 높아지며, 콜백 지옥에 빠지게 된다.


## Promise 생성자 함수

```javascript
const promise = new Promise(function(resolve, reject) {
    // 코드
})
```

- Promise 생성자 함수를 new 연산자와 함께 호출하면 Promise 객체를 생성한다.
- 비동기 처리를 수행할 콜백함수를 인수로 전달받고, 이 콜백 함수는 `resolve(value), reject(error)` 함수를 인수로 전달받는다.



### Promise 객체

Promise는 비동기 처리 상태와 결과를 관리하는 객체로 state, result 내부 프로퍼티를 가진다. 내부 프로퍼티이므로 개발자가 직접 접근하는 것은 불가능하고, `then, catch, finally` 메서드를 사용하여 접근 가능하다.

- state : 비동기 처리가 어떻게 진행되고 있는지 상태

  - pending : 프로미스가 생성된 직후 기본 상태로 아직 수행되지 않은 상태를 의미함(보류)
  - fulfilled : resolve가 호출(성공)
  - rejected : reject가 호출(실패)

- result : 처음에 `undefined`, resolve 함수가 호출되면 value, reject 함수가 호출되면 error로 변한다.

```javascript
const fulfilled = new Promise((resolve) => resolve(1));

console.log(fulfilled);
```
<img width="287" alt="스크린샷 2023-07-03 오후 6 16 37" src="https://github.com/sejineeee/ModernJavascript/assets/86041335/07cbd525-1aa6-4bd7-8a93-ad479a32c2a1">

```javascript
const rejected = new Promise((resolve, reject) => reject(new Error('error occured')));

console.log(rejected);
```
<img width="449" alt="스크린샷 2023-07-03 오후 6 16 55" src="https://github.com/sejineeee/ModernJavascript/assets/86041335/9430e0e8-f2bc-49d5-b73e-bf6814b871c1">

### then

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('완료!'), 1000);
})

promise.then(
    (response) => console.log(response),
    (error) => console.log(error));
```

- 첫 번째 콜백함수는 fulfilled 상태가 되면 호출. 이때 콜백 함수는 프로미스의 처리 결과를 인수로 전달받음.

- 두 번째 콜백함수는 rejected 상태가 되면 호출. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받음.

### catch

한 개의 콜백함수를 인수로 전달 받음. 프로미스가 rejected 상태인 경우만 호출됨

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error('rejected!!')), 1000)
})

promise.catch((error) => console.log(error));
```

### finally

한 개의 콜백함수를 인수로 전달 받음. fulfilled, rejected 상태와 상관없이 무조건 한 번 호출되기 때문에 공통적으로 수행해야 할 처리 내용이 있을 때 사용하면 유용하다.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('완료!')
  }, 1000)
})

promise.finally(() => {
  console.log('Promise.finally');
});
```

### then ... catch

```javascript
const promise = fetch('https://jsonplaceholder.typicode.com/todos/1');

promise.then(
    (response) => {
    console.log(response);
    }).catch((error) => {console.log(error)});
```

#### 문제

Promise에 대해서 조금 더 이해하기 위해서 [모던 자바스크립트 튜토리얼](ko.javascript.info/promise-basics)에 나와있는 문제를 풀어보았다.

- 프라미스로 지연 만들기
```javascript
function delay(ms) {
  return new Promise((resolve) => {
    return setTimeout(resolve, ms)
  })
}

delay(5000).then(() => alert('3초후 실행'))
```

- 프라미스로 원 만들기
```javascript
const div = document.querySelector('div');
div.style.backgroundColor = 'red';

function showCircle(width, height, borderRadius) {
  return new Promise(resolve => {
    setTimeout(() => {
      div.style.width = `${width}px`;
      div.style.height = `${height}px`;
      div.style.borderRadius = `${borderRadius}%`;
      
      resolve(div);
    }, 0)
  })
}

showCircle(150, 150, 100).then(div => {
  div.classList.add('message-ball');
  div.append('hello, world!');
})
```

promise 내부 코드 작성하는 건 너무 어렵다. `div.style.width` 또는 `return new Promise()`를 하는 건 혼자의 힘으로 작성할 수 있었으나, 내부 코드에 setTimeout과 resolve에 div를 전달해야 된다는 것은 해답을 보고 풀 수밖에 없었다. Promise랑 언제쯤 친해질 수 있을까...

- 프라미스 : then vs catch

```javascript
// 두 코드가 동일하게 동작할까?
promise.then(f1).catch(f2);

promise.then(f1, f2);
```

동일하게 동작할 것 같다. then에는 두 개의 콜백함수 인자를 받는데, 첫 번째 인자는 fulfilled 됐을 때 호출되는 함수, 두 번째 인자는 rejected 됐을 때 호출이 된다. 그렇기 때문에 f1은 fulfilled 됐을 때, f2는 rejected 됐을 때 호출이 된다.

then.catch도 then이 fulfilled 됐을 때 f1이 호출되고, rejected가 됐을 때 catch에서 호출될 것이라고 생각해서 두 코드는 동일하게 동작할 것이라고 생각했다.


```
답은!!!! 두 코드는 다르게 동작한다고 한다!!!!
promise.then(f1).catch(f2) 코드의 경우 f1에서 에러가 발생할 경우 catch 구문에서 에러가 처리된다.

promise.then(f1, f2) 코드의 경우 then 핸들러에서 에러가 발생하면 체인 아래로 전달이 되는데, 이어지는 체인이 없기 때문에 f1에서 발생한 에러를 처리하지 못하는 것이다.
```

---
참고
- [모던 자바스크립트 튜토리얼](ko.javascript.info/promise-basics)