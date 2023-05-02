# 클로저

## 클로저와 렉시컬 환경

```javascript
const x = 1;

function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  };
  return inner;
}

const innerFunc = outer();
innerFunc(); // 10

// outer 함수 호출시 중첩함수 inner를 반환하고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거됨
// outer 함수의 실행 컨텍스트가 스택에서 제거되어도 inner 함수는 outer 함수의 지역 변수를 참조하고 있음
```

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료된 외부 함수의 변수를 참조할 수 있다. 이런 중첩함수를 `클로저(closure)`라고 한다.

outer 함수는 실행 컨텍스트 스택에서 제거되지만 렉시컬 환경까지 소멸하는 것은 아니다. outer 함수의 렉시컬 환경은 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않는다.

클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고, 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이고, 클로저에 의해 참조된 상위 스코프의 식별자를 자유 변수라고 부른다.

```plaintext
💡 메모리 낭비?
상위 스코프의 식별자 중에서 참조하고 있는 식별자만 기억하고, 참조하고 있지 않은 식별자는 기억하지 않기 때문에 불필요한 메모리 낭비라고 볼 수 없다!
```

### 클로저 활용

클로저는 상태(state)가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용된다.

외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안전성을 높이기 위해 클로저는 적극적으로 사용된다.

```javascript
// 함수형 프로그래밍에서 클로저를 활용 예시
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 자유 변수 counter를 기억하는 클로저를 반환
  return function () {
    counter = aux(counter);
    return counter;
  };
}

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

const increaser = makeCounter(increase);
const decreaser = makeCounter(decrease);

increaser(); // 1
decreaser(); // -1

// makeCounter 함수를 호출해서 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 가진다
// increaser, decreaser에 할당된 함수는 각각 독립된 렉시컬 환경을 가져서 자유변수를 공유하지 않아 카운터의 증감이 연동되지 않음
```

```javascript
// 자유 변수를 공유하여 카운터의 증감이 연동되도록 하려면
const counter = (function () {
  let counter = 0;

  return function (aux) {
    counter = aux(counter);
    return counter;
  };
})();

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

counter(increase); // 1
counter(increase); // 2
counter(decrease); // 1
counter(decrease); // 0
```

## 캡슐화와 정보 은닉

- 캡슐화 : 프로퍼티와 메서드를 하나로 묶는 것

- 정보 은닉 : 객체의 특정 프로퍼티나 메서드를 감출 목적으로 캡슐화를 사용하는 것으로 객체의 상태가 변경되는 것을 방지하여 정보를 보호하고, 결합도를 낮추는 효과가 있음

객체의 모든 프로퍼티와 메서드는 기본적으로 public 하다.

2021년 [TC39 프로세스의 state 3](https://github.com/tc39/proposal-class-fields#private-fields)에는 Class에 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다.

---

참고

- [private field](https://github.com/tc39/proposal-class-fields#private-fields)
