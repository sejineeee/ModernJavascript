# 비동기 프로그래밍

자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 가지므로, 한번에 하나의 태스크만 실행할 수 있는 싱글스레드 방식으로 동작한다. 비동기처리는 현재 실행 중인 태스크가 종료되지 않은 상태라 하더라도 다음 태스크를 곧바로 실행하므로 블로킹이 발생하지 않는다는 장점이 있지만, 실행 순서가 보장되지 않는 단점도 있다.

## 이벤트 루프와 태스크 큐

- 이벤트 루프:  태스크가 들어오길 기다렸다가 태스크가 들어오면 이를 처리하고, 처리할 태스크가 없는 경우에는 잠들고, 끊임없이 돌아가는 자바스크립트 내 루프이다. (참고 - [모던 자바스크립트 튜토리얼](https://ko.javascript.info/event-loop))

- 태스크 큐 : 콜백함수들이 대기하는 큐(FIFO: First-in, First-out) 형태의 배열

비동기 함수인 setTimeout의 콜백함수는 태스크 큐에 푸시되어 대기하다가 콜스택이 비게 되면, 즉 전역 코드 및 명시적으로 호출된 함수가 모두 종료하면 비로소 콜 스택에 푸시되어 실행된다.


```javascript
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"), 500);
const baz = () => console.log("Third");

bar();
foo();
baz();

// 500ms 후에 콜백함수가 호출스택에 추가된다는 것을 의미하는 게 아님! 500ms 후에 태스크 큐에 추가됨.
```

![이벤트 루프와 태스크 큐](https://res.cloudinary.com/practicaldev/image/fetch/s--dhjH4Wt---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif14.1.gif)
사진 출처 : Lydia Hallie

1. bar 함수를 호출함. bar는 `setTimeout` 함수를 반환함.
2. `setTimeout` 콜백 함수는 WEB API에 추가되고, `setTimeout`과 `bar` 함수는 콜스택으로부터 튀어나오게 됨
3. 타이머가 실행되고, 그동안 `foo`가 호출되어 콘솔에 출력됨. return 구문이 없기 때문에 반환값은 `undefined`임. 이어서 `baz`가 호출되고, 콜백함수는 큐에 추가됨.
4. `baz`의 콘솔이 출력됨. 이벤트 루프는 콜스택이 비었음을 확인하고, 큐에 있는 콜백함수를 콜스택에 추가함.
5. 콜백함수의 콘솔이 출력됨

---

REST API, Promise, async/await 같은 경우는 따로 한 챕터씩 정리할 예정이다.

---
참고

- [모던 자바스크립트 튜토리얼](https://ko.javascript.info/event-loop)

- [chanmi님 블로그](https://chanmi-lee.github.io/articles/2020-06/JavaScript-Visualized-Event-Loop)