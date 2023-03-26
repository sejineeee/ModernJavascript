# 함수

### 함수 정의

#### 함수 선언문
함수 선언문은 표현식이 아닌 문이기 때문에 변수에 할당할 수 없고, 익명함수로 선언할 수 없다.

```javascript
function add(x, y) {
    return x + y;
}
```

하지만 아래 코드를 보면, 변수에 할당되는 것처럼 보인다..?
함수 선언문은 표현식이 아니기 때문에 변수에 할당할 수 없다고 했는데 어떻게 된 거지?

```javascript
const plus = function add(x, y) {
    return x + y;
}

plus(3, 4); // 7

// add 식별자로는 접근이 불가능함
```
단독으로 사용할 경우에는 자바스크립트 엔진이 선언문으로 해석하고, 피연산자로 사용될 경우 함수 리터럴 표현식으로 해석되기 때문에 변수에 할당된 것이다.


함수 리터럴에서 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자이기 때문에 외부에서는 접근이 불가능하다. 하지만 함수 선언문의 경우, 자바스크립트 엔진에서 암묵적으로 함수 선언문의 식별자 이름과 동일한 이름의 식별자를 생성하고, 함수 객체를 할당한다.

```javascript
const sayHello = function sayHello() {
    console.log('안녕');
}
```

#### 함수 표현식

```javascript
const minus = function(x, y){
    return x - y;
};

minus(7, 2); // 5
```
함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다.

#### 함수 선언문과 함수 표현식의 차이점

함수 선언문과 함수 표현식은 호이스팅에서 차이점이 있다.

```javascript
add(3, 4); // 호이스팅 됨
sub(4, 3); // ReferenceError: sub is not defined 

function add(x, y) {
  return x + y;
}

const sub = function(x, y) {
  return x - y;
}
```
함수 선언문은 런타임 이전에 함수 객체가 먼저 생성되고, 자바스크립트 엔진이 암묵적으로 식별자를 생성하고 함수 객체를 할당한다. 그렇기 때문에 런타임 시점에는 이미 함수 객체가 생성되어 있는 상태이고, 선언문의 이름과 동일한 식별자에 할당까지 완료가 된 상태이다.

함수 선언문은 호이스팅이 발생하여 선언문 이전에 참조 및 호출이 가능한 것이다. 함수 선언 후 호출을 하는 것이 규칙이기 때문에 <mark>함수 표현식을 사용할 것을 권장한다.</mark>

#### Function 생성자 함수

`Function()` 생성자는 새로운 함수 객체를 생성한다. 이를 통해서 만든 함수는 클로저를 생성하지 않는다.

```javascript
const add1 = (function() {
  const a = 10;
  return function(x, y) {
    return x + y + a;
  };
}());

console.log(add1(1, 2));
// 외부 함수에 있는 a 식별자를 참조할 수 있다.

const add2 = (function() {
  const a = 10;
  return new Function('x', 'y', 'return x + y + a');
}());

console.log(add2(1, 2));
// ReferenceError: a is not defined
// Function 생성자로 만든 함수는 클로저를 생성하지 않기 때문에
```


#### 화살표 함수

- 화살표 함수는 생성자 함수로 사용할 수 없다.

- this 바인딩 방식이 다르다. (화살표 함수는 실행 컨텍스트 생성시 this를 바인딩하는 과정이 제외됐다. 화살표 함수 내부에는 this가 아예 없으며, 접근하고자 하면 스코프 체인상 가장 가까운 this에 접근하게 됨)

- prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않는다.

```javascript
const add = function(x, y) {
  console.log(arguments);
  return x + y;
}

console.log(add(3, 4, 5))
/*
{
  '0': 3,
  '1': 4,
  '2': 5,
  length: 3,
  callee: ƒ add(),
  __proto__: Object
}
*/

const add2 = (x, y) => {
  console.log(arguments);
  return x + y;
}

add2(1, 2, 3); // ReferenceError: arguments is not defined
// 화살표 함수는 arguments 객체를 생성하지 않음
```

### 함수 호출

함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 생성되고 일반 변수와 마찬가지로 undefined로 초기화된 이후 인수가 순서대로 할당된다.

자바스크립트에서 함수는 매개변수의 개수와 인수의 개수가 일치하는지 체크하지 않는다. 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 일반적이지만 그렇지 않은 경우에도 에러 발생하지 않음.


```typescript
// typescript
const add = (x: number, y: number): number => {
    return x + y;
}

add(1, 2, 4);
```
타입스크립트를 사용하면 arguments가 초과하거나 부족할 경우 에러가 발생한다.

ES6에서 도입되어 매개변수에 기본값을 지정할 수 있다.
```javascript
//
const add = function(x, y) {
  console.log(arguments);
  return x + y;
}
```

### 재귀 함수

재귀 함수는 자기 자신을 호출하는 함수이다. 재귀함수를 사용하면 반복 처리를 반복문 없이 구현할 수 있다. 반복문보다 재귀함수를 사용하는 편이 더 이해하기 쉬울 경우에만 사용하는 것이 좋다.

```javascript
function countdown(n) {
  if(n < 0) return;
  console.log(n);
  countdown(n - 1);
}

countdown(10);
```
재귀호출을 멈출 수 있는 탈출 조건을 만들지 않으면 무한 루프에 빠지게 된다.

### 콜백 함수
함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백함수라고 한다.


### 순수 함수와 비순수 함수

- 순수함수
  - 외부 상태에 의존하지도 않고 변경하지도 않는(부수 효과❌) 함수
  - 일반적으로 최소 하나 이상의 인수를 전달받음
  - 인수의 불변성 유지

  ```javascript
  let number = 0;

  // 동일한 인수가 전달되면 언제나 동일한 값을 반환
  function increaseNumber(n) {
    return ++n;
  }

  // 순수 함수가 반환한 값을 변수에 재할당한 것임
  number = increaseNumber(number);
  console.log(number);

  number = increaseNumber(number);
  console.log(number);
  ```

<br />

- 비순수 함수
  - 외부상태에 의존하거나 외부 상태를 변경하는(부수 효과⭕️) 함수

  ```javascript
  let count = 0;

  // 외부에 있는 변수에 의존하며 외부 상태를 변경함
  function increase() {
    return ++count;
  }
  increase();
  console.log(count);

  ```

함수가 외부 상태를 변경하면 상태 변화를 추적하기 어려워지기 때문에 순수함수를 사용하는 것이 좋다.

함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 외부상태를 변경하는 부수효과를 최소화해서 불변성을 지향하는 프로그래밍 패러다임이다.
