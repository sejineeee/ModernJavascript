# 타입 변환

타입 변환은 명시적 타입 변환(타입 캐스팅)과 암묵적 타입 변환(타입 강제 변환)이 있다.

### 암묵적 타입 변환(타입 강제 변환)
```javascript
1 + '2' // '12'
NaN + '' // 'NaN'
true + '' // 'true'
[] + '' // ''
[1, 2, 3] + '' // '1, 2, 3'
console.log({} + ''); // '[object Object]'
```

```javascript
1 - '1'; // 0
+ '' // 0
+ '1'; // 1
+ true; // 1
+ null; // 0

+ undefined; // NaN
+ {}; // NaN
+ []; // 0
+ [10, 20]; // NaN
```

자바스크립트 엔진은 boolean 타입이 아닌 값을 truthy, falsy로 구분한다. falsy 외에는 모두 truthy이기 때문에 falsy 값만 알아두면 된다.

`false, 0, -0, undefined, null, NaN, ''`

### 명시적 타입 변환

#### 문자열 타입 변환
```javascript
String(NaN); // 'NaN'
String(Infinity); // 'Infinity'
String(false); // 'false'

(NaN).toString(); // 'NaN'
(Infinity).toString(); // 'Infinity'
(false).toString(); // 'false'
```

#### 숫자 타입 변환
```javascript
Number('10.53'); // 10.53
+ '10.53' // 10.53

parseInt('0'); // 0
parseFloat('10.53'); // 10.53
parseInt('10.53'); // 10 

'0' * 1; // 1
true * 4; // 4
```

#### 불리언 타입 변환

```javascript
Boolean('N'); // true
Boolean(''); // false
Boolean(0); // false
Boolean({}); // true
Boolean([]); // true
!![]; // true
!!0; // false
```
