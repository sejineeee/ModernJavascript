# 데이터 타입

데이터 타입은 원시타입과 객체 타입으로 분류할 수 있다. 원시타입에는 number, string, boolean, undefined, null, symbol, bigint 타입이 있고, 객체 타입에는 객체, 함수, 배열 등이 있다.

## 원시타입
### number
숫자는 배정밀도 64비트 부동소수점 형식을 따른다.
```plaintext
배정밀도 64비트 부동소수점이란?
52비트는 숫자를 저장, 11비트는 소수점 위치, 1비트는 양수/ 음수 부호를 저장한다.
```

숫자는 10진수로 해석이 되고, 모두 실수로 처리한다.
```javascript
console.log(1 === 1.0); // true

```

Infinity, -Infinity, NaN도 number type이다.
```javascript
let a = 1 * hello; // NaN
console.log(typeof a); // number

let b = 10 / 0; // Infinity
console.log(typeof b); // number

let c = 10 / -0; // -Infinity
console.log(typeof c); // number
```

### string
16비트 유니코드 문자(UTF-16)의 집합으로 전 세계 대부분의 문자 표현이 가능하다. `'', "", ``,`

```javascript
let d = "세진이는 "안녕"이라고 말했다";
// 이렇게 쓸 경우는 세진이는 까지만 string이라고 인식을 하고, 뒷 부분 안녕은 식별자로 인식하여 찾지 못한다고 에러가 발생한다.

// 이런식으로 바꿔줄 수 있다.
d = '세진이는 "안녕"이라고 말했다';
d = "세진이는 \"안녕\"이라고 말했다"; // 이스케이프 시퀀스를 사용함
```

문자열 내에서 줄바꿈은 허용되지 않기 때문에 `\n` 이스케이프 시퀀스를 사용한다.
```javascript
let e = '안\n녕';
```

### boolean
true, false 값

### undefined
undefined는 javascript 엔진이 변수를 초기화 할 때 사용하는 값.

undefined 값은 자바스크립트 엔진이 변수를 초기화하는데 사용하기 때문에 변수에 의도적으로 undefined 값을 할당한다면 혼란을 줄 수 있다.

### null
변수에 값이 없다는 것을 의도적으로 명시할 때 null 값을 할당한다.

### symbol
변경 불가능한 원시 타입의 값으로 다른 값과 중복되지 않는 유일무이한 값이다.

이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.

```javascript
Symbol('foo') === Symbol('foo'); // false

let obj = {};

const foo = Symbol('foo');
const bar = Symbol('bar');

obj[foo] = 'foo~~';
obj[bar] = 'bar~~';

console.log(obj[foo]); // 'foo~~'
console.log(Object.hasOwn(obj, 'foo')); // false
```

### bigint

원시 값이 안정적으로 나타낼 수 있는 최대치인 2^53 - 1(Number.MAX_SAFE_INTEGER)보다 큰 정수를 표현할 수 있는 내장 객체.

정수 뒤에 n을 붙이거나 BigInt() 함수를 호출해서 생성할 수 있다.

```javascript
let a = 9007199254740991n;

let b = BigInt(9007199254740991);
```

## 데이터 타입의 필요성

- 데이터 타입에 따라 확보되는 메모리 공간의 크기가 달라진다.

- 값을 참조할 때, 읽어들여야 할 메모리 셀의 개수(바이트 수)를 알아야 된다. 데이터 타입에 의해서 알 수 있다.

- 데이터 타입에 의해서 2진수를 어떻게 해석할 지 결정할 수 있다.


## 동적 타입 언어

자바스크립트의 변수는 할당에 의해 타입이 결정되고, 재할당에 의해서 타입은 언제든지 변할 수 있다. 자바스크립트 엔진에 의해 암묵적으로 타입이 자동으로 변환되기도 한다.

```javascript
let a = null;

console.log(typeof a); // object
```
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#typeof_null

이러한 오류를 고치려고 했으나 기존에 이러한 방식으로 사용한 것들이 존재하기 때문에 수정하는 것이 거절됨.

정적 타입 언어는 변수를 선언할 때, 타입을 사전에 선언해야 된다. 컴파일 시점에 타입 체크를 하기 때문에 런타임에 발생하는 에러를 줄일 수 있다.


Typescript는 정적 타입 언어로 타입 구문이 존재하는 Javascript이다. Javascript를 사용했을 때, 런타임에서 발생할 수 있는 에러를 Typescript를 사용하면 줄일 수 있다.

```javascript
function compact(arr) {
    if (orr.length > 10) {
        return arr.trim(0, 10);
    }
    return arr;
}
// 조건문에 arr가 orr로 오타가 발생했지만, javascript 파일 편집기에서는 에러가 발생하지 않는다.
// 런타임에서 에러가 발생함
```

```javascript
// @ts-check

function compact(arr) {
  if (orr.length > 10) {
      return arr.trim(0, 10);
  }
  return arr;
}
```
`@ts-check`를 통해서 오류를 검출할 수 있음

<img width="543" alt="스크린샷 2023-03-10 오후 7 24 56" src="https://user-images.githubusercontent.com/86041335/224292329-ff5ea04d-3891-4ee8-b00f-13c5d7e8bfc8.png">