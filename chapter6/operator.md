# 연산자
## 산술 연산자
피연산자(연산의 대상)의 개수에 따라 이항 산술 연산자와 단항 산술 연산자로 나눌 수 있다.

### 이항 산술 연산자
 `+, -, *, /, %`

피연산자의 값을 바꾸는 부수효과 없음.

```javascript
let x = 1 + true;
// true가 1로 boolean에서 number 타입으로 변환

console.log(x) // 2

x = 1 + null;
// null은 0 타입으로 변환

console.log(x) // 1

x = 1 + undefined;
console.log(x); // NaN
// undefined는 타입 변환되지 않음
```

### 단항 산술 연산자
단항 산술 연산자는 1개의 피연산자를 산술 연산하여 값을 만든다.

`++, --`

```javascript
// 선증가 후 할당(선행 연산자)
++a; 
// 선감소 후 할당(선행 연산자)
--a;
// 선할당 후 증가(후행 연산자)
a++;
// 선할당 후 감소(후행 연산자)
a--;
```
선행 연산자와 후행 연산자는 피연산자의 값을 변경하는 부수효과가 있다.

`+, -`
단항 연산자는 숫자 타입으로 변환하여 값을 반환하거나 피연산자의 부호를 반전한다.
```javascript
let a = true;
console.log(+a); // 1
// true 값은 1

let b = false;
console.log(+b); // 0

let c = 'hi';
console.log(+c); // NaN

console.log(a); // true
// a 자체의 값은 변하지 않았다.

console.log(-a); // -1
console.log(-b) // 0
console.log(-c) // NaN
```

## 할당 연산자

```javascript
let a = 10;

a += 5;
console.log(a); // 15
// a = a + 5와 같음

a -= 3;
console.log(a); // 12

a *= 2;
console.log(a); // 24

a /= 3;
console.log(a); // 8

a %= 3;
console.log(a); // 2
```

문자열에도 할당 연산이 가능하다. (문자열을 연결할 수 있는 연산자 `+`의 할당 연산자인 `+=`만 가능)
```javascript
let str = 'hi';

str += '😎';
console.log(str); // 'hi😎'
```

## 비교 연산자

`==, !=`는 암묵적으로 타입 변환을 시켜 비교하기 때문에 사용 X

`===, !==`는 암묵적 타입 변환을 시키지 않고 비교하므로 이 연산자를 사용해야 된다.

예외 )
NaN은 자신과 일치하지 않는 유일한 값이기 때문에 일치 연산자로 비교할 수 없다.
`Number.isNaN` 메소드를 사용해서 NaN을 확인한다.
```javascript
console.log(NaN === NaN) // false

let a = 'hello' * 1; // NaN

console.log(a === NaN);
console.log(Number.isNaN(a));
```
`Object.is()`는 2개의 값이 같은 값인지 비교하여 boolean 값을 반환하는 메소드이다.
```javascript
console.log(0 === -0); // true
Object.is(0, -0) // false

// NaN도 Object.is 메서드를 통해 비교할 수 있다.
let a = 'hello' * 1; // NaN

Object.is(a, NaN); // true
```

## 삼항 연산자
`조건식 ? true일 때 반환 : false일 때 반환`

삼항 연산자는 `if ... else`문과 다르게 값처럼 사용할 수 있다.

```javascript
let x = 10;

let result = if (x % 2) { result = '홀수'} else { result = '짝수'};
// SyntaxError: Unexpected token

let result = x % 2 ? '홀수' : '짝수';
console.log(result); // 짝수
// 0은 false로 암묵적 타입 변환되어 짝수가 출력
```

## 논리 연산자
`||` or 연산자
```javascript
true || false; // true
false || false; // false

'Cat' || 'Dog' // Cat
```

`&&` and 연산자
```javascript
true && false; // false
true && true; // true
'Cat' && 'Dog';; // Dog
```

`!` Not 연산자
```javascript
!true // false
!false // true

let a = 'Cat';
console.log(!a); // false
```

## 지수 연산자

`**`는 거듭제곱 연산자

BigInts를 피연산자로 받는 것을 제외하고는 `Math.pow()` 메서드와 같다.

```javascript
let a = 2 ** 3;
console.log(a) // 8

Math.pow(2, 2); // 4
Math.pow(2, 3); // 8
```
```javascript
1234n ** 12n; // 12467572902176589255564000298710470656n

Math.pow(1234n, 12n); // TypeError: Cannot convert a BigInt value to a number

```
음수를 거듭제곱으로 사용하려면 `()` 괄호로 묶어야 된다.
```javascript
let a = (-5) ** 2;

console.log(a); // 25
```

지수 연산자는 이항 연산자 중 우선순위가 가장 높기 때문에 `()` 그룹 연산자를 이용해서 우선순위를 조절해줘야 된다.

## 그 외 연산자
쉼표 연산자, 그룹 연산자, typeof 연산자, 옵셔널 체이닝, nullish 병합 연산자, delete, new, instaneof, in 등이 있다.
(다른 chapter에서 연관지어 설명할 예정)
