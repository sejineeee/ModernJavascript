# 배열

자바스크립트의 배열은 해시 테이블로 구현된 객체로, 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느리지만, 특정 요소를 검색하거나 요소 삽입 또는 삭제하는 경우에는 빠른 성능을 기대할 수 있다.

```plaintext
해시 테이블은 연관 배열(associative array)의 구현으로, 키-값 쌍의 목록으로 이루어져 있으며, 키를 통해 값을 검색할 수 있는 자료 구조입니다. 내부적으로 해시 함수를 사용하여 키 값을 메모리에서 값이 저장된 위치를 가리키는 인덱스로 변환합니다. 해시 테이블은 빠른 검색, 삽입 및 삭제 연산을 제공합니다.
```

## 배열 생성

배열 리터럴, Array 생성자 함수, `Array.of`, `Array.from` 방법이 있다.

```javascript
const arr = [1, 2, 3];
const arr2 = new Array(3);
console.log(arr2); // [ <3 empty items>]
const arr3 = new Array(4, 5, 6);
console.log(arr3); // [4, 5, 6]
const arr4 = Array.of(7, 8, 9);
console.log(arr4); // [7, 8, 0]
const arr5 = Array.from('foo');
console.log(arr5); // ['f', 'o', 'o']
const arr6 = Array.from({ length: 3, 0: 'apple', 1: 'banana', 2: 'cherry' });
console.log(arr6);
const arr7 = Array.from([1, 2, 3], (x) => x * 3);
console.log(arr7); // [3, 6, 9]
```

`Array.from` 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달 받아 배열로 반환함

```
Array.from(arrayLike, mapFn)

유사배열객체 : 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체로 for문으로 순회할 수 있음

이터러블객체 : Symbol.iterator 메서드를 구현하여 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있는 객체
```

## 배열 메서드

`Array.isArray` : 전달된 인수가 배열인지 아닌지 boolean 값을 반환

`Array.prototype.indexOf` : 인수로 전달된 요소를 검색하여 인덱스를 반환, 검색시 존재하지 않으면 -1 값을 반환

```javascript
const fruits = ['apple', 'kiwi', 'banana'];
Array.isArray(fruits); // true

fruits.indexOf('kiwi'); // 1

if (fruits.indexOf('pineapple') === -1) {
  fruits.push('pineapple');
}

console.log(fruits); // [ 'apple', 'kiwi', 'banana', 'pineapple' ]

// includes 이용해서 해당 요소를 포함하고 있는지 확인하는게 가독성 좋음!
if (!fruits.includes('pineapple')) {
  fruits.push('pineapple');
}

console.log(fruits);
```

### 원본 배열을 변경하는 메서드

`Array.prototype.push` : 원본 배열의 마지막 요소로 추가하고 변경된 length 값을 반환함

`Array.prototype.pop` : 원본 배열의 마지막 요소를 제거하고 제거한 요소를 반환함

`Array.prototype.unshift` : 원본 배열의 첫 번째에 요소를 추가하고, 변경된 length 값을 반환함

`Array.prototype.shift` : 첫 번째 요소를 제거하고 제거한 요소를 반환함

`Array.prototype.splice` : 중간에 요소를 추가하거나 중간에 있는 요소를 제거하고, 제거한 요소를 반환함

```javascript
const fruits = ['apple', 'kiwi', 'banana'];

// push

console.log(fruits.push('orange')); // 4  변경된 length 값을 반환하기 때문
console.log(fruits); // [ 'apple', 'kiwi', 'banana', 'orange' ]

// spread operator를 사용하면 성능👍🏻
const newFruits = [...fruits, 'orange'];
console.log(newFruits); // [ 'apple', 'kiwi', 'banana', 'orange' ]

// pop
console.log(fruits.pop()); // 'banana'
console.log(fruits); // ['apple', 'kiwi']

// unshift
console.log(fruits.unshift('grape')); // 4  변경된 length 값을 반환함
console.log(fruits); // [ 'grape', 'apple', 'kiwi', 'banana' ]

// spread operator를 사용하면 성능👍🏻
const unshiftFruits = ['grape', ...fruits];
console.log(unshiftFruits);

// shift
console.log(fruits.shift()); // 'apple'
console.log(fruits); // ['kiwi', 'banana']
```

```javascript
// splice
// Array.prototype.splice(start, deleteCount, addItems)
// 첫번째 매개변수만 필수, 나머지 매개변수는 옵션

const fruits = ['apple', 'kiwi', 'banana', 'orange', 'grape'];

const removeFruits = fruits.splice(2, 2, 'gold kiwi');
console.log(removeFruits); // [ 'banana', 'orange' ]
console.log(fruits); // [ 'apple', 'kiwi', 'gold kiwi', 'grape' ]
```

`Array.prototype.reverse` : 원본 배열의 순서를 반대로 뒤집어 원본 배열이 변경된다. 반환값은 변경된 배열이다.

`Array.prototype.fill` : 인수로 전달받은 값을 배열의 요소로 채움

```javascript
const arr = [1, 2, 3, 4];

arr.reverse();
console.log(arr); // [4, 3, 2, 1]

// 두번째 인수로 요소 채우기 시작할 인덱스
// 세번째 인수로 요소 채우기를 멈출 인덱스(미포함)
arr.fill(0, 1, 3);
console.log(arr); // [4, 0, 0, 1]
```

### 원본 배열을 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드

`Array.prototype.concat` : 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환함

```javascript
const fruits = ['apple', 'kiwi', 'banana'];

const newFruits = fruits.concat(['orange', 'pineapple']);

console.log(newFruits); // [ 'apple', 'kiwi', 'banana', 'orange', 'pineapple' ]

const unshiftFruits = ['grape', 'cherry'].concat(fruits);

console.log(unshiftFruits); // [ 'grape', 'cherry', 'apple', 'kiwi', 'banana' ]

// spread 문법 사용 권장👍🏻
const spreadFruits = [...['apple', 'kiwi'], ...['banana', 'orange']];
console.log(spreadFruits); // [ 'apple', 'kiwi', 'banana', 'orange' ]
```

`concat` 메서드는 ES6의 spread 문법으로 대체할 수 있으며, spread 문법을 일관성 있게 사용하는 것을 권장함

`Array.prototype.slice` : 인수로 전달된 범위의 요소들을 복사하여 배열로 반환함

```javascript
// Array.prototype.slice(start, end)
// start는 복사를 시작할 인덱스. 음수인 경우 배열의 끝에서의 인덱스를 나타냄
// end는 복사를 종료할 인덱스로 해당 인덱스의 요소는 복사되지 않음. 생략시 length 값이 기본값

const fruits = ['apple', 'kiwi', 'banana', 'orange', 'grape'];

const sliceFruits = fruits.slice(2);
console.log(sliceFruits); // [ 'banana', 'orange', 'grape' ]

const threeFruits = fruits.slice(1, 3);
console.log(threeFruits); // [ 'kiwi', 'banana' ]

const minusFruits = fruits.slice(-2);
console.log(minusFruits); // [ 'orange', 'grape' ]
```

`Array.prototype.join` : 인수로 문자열을 전달하면 전달한 문자열을 구분자로 연결한 문자열을 반환함. (인수 전달 안 해도 됨)

```javascript
const str = ['j', 'o', 'i', 'n'];

const joinArr = str.join();
console.log(joinArr); // 'j,o,i,n'
```

`Array.prototype.includes` : 배열 내에 특정 요소가 포함되어 있는지 확인하여 boolean 값을 반환

```javascript
// 첫 번째 인수로 검색할 대상 지정
// 두 번째 인수로 검색을 시작할 인덱스 지정. 음수를 전달할 경우 length + index 계산 값으로 지정됨

const fruits = ['apple', 'kiwi', 'banana', 'orange', 'grape'];

fruits.includes('kiwi', 2); // false
```

`Array.prototype.flat` : 중첩배열을 평탄화함

```javascript
// 인수 생략시 기본값 1임. Infinity를 전달하면 중첩배열 모두 평탄화함.
const arr = [1, [2, [3], 4], 5];
const flatArr = arr.flat(Infinity);

console.log(flatArr);
console.log(arr);
```

### 배열 고차 함수

`Array.prototype.sort(compareFn)` : 원본 배열을 변경하면 정렬된 배열을 반환함(기본 오름차순)

compareFn 반환값

- 0보다 작은 경우 : 첫 번째 요소가 두 번째 요소보다 작음. 두 요소의 순서는 유지
- 0인 경우 : 두 요소는 동일하므로 정렬하지 않음
- 0보다 큰 경우 : 첫 번째 요소가 두 번째 요소보다 큼. 두 요소의 순서가 반전

```javascript
const str = ['banana', 'apple', 'tomato', 'carrot'];

str.sort(); // [ 'apple', 'banana', 'carrot', 'tomato' ]

const points = [40, 100, 1, 5, 2, 25, 10];

points.sort((a, b) => a - b); // [ 1, 2, 5, 10, 25, 40, 100 ]

const todos = [
  { id: 4, content: 'Javascript' },
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' },
];

function compare(key) {
  return (a, b) => (a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0);
}
// const a = 'HTML' > 'CSS'; HTML이 CSS보다 커서 true 값이 나옴

todos.sort(compare('id')); // id 순서별로 정렬됨
todos.sort(compare('content')); // content 순서별로 정렬됨
```

`Array.prototype.forEach(callbackFn)` : 배열을 순회하면서 콜백함수를 반복 호출함. undefined를 반환함

```javascript
const numbers = [1, 2, 3];
const pows = [];

const returnedValue = numbers.forEach((number) => {
  pows.push(number ** 3);
});

console.log(returnedValue); // undefined
console.log(pows); // [1, 8, 27]

// 원본 배열을 변경하고 싶은 경우?
numberArr.forEach((number, i, arr) => {
  arr[i] = number ** 3;
});

console.log(numberArr); // [1, 8, 27]
```

`Array.prototype.map(callbackFn)` : 배열의 모든 요소를 순회하면서 콜백함수를 반복 호출하여 반환값들로 새로운 배열을 반환함. 원본 배열 변경하지 않음!

```javascript
const numbers = [4, 9, 16];

const roots = numbers.map((number) => {
  return Math.sqrt(number);
});

console.log(roots); // [ 2, 3, 4 ]
console.log(numbers); // [ 4, 9, 16 ]
```

❗️forEach는 undefined를 반환, map은 콜백 함수의 반환값들로 구성된 배열을 반환하는 차이가 있음

`Array.prototype.filter` : 배열의 모든 요소를 순회하면서 콜백함수를 반복 호출하여 반환값이 true인 요소로만 구성된 배열을 반환함. 원본배열 변경되지 않음.

```javascript
const numbers = [1, 2, 3, 4, 5];

const odds = numbers.filter((number) => {
  return number % 2;
});

console.log(odds); // [1, 3, 5]
```

`Array.prototype.reduce(callbackFn, initialValue)` : 배열의 모든 요소를 순회하며 콜백함수를 반복 호출하고 반환값을 다음 순회 시에 콜백함수의 첫 번째 인수로 전달하면서 콜백함수를 호출하여 하나의 결과값을 만들어 반환함
(initialValue는 생략 가능하지만, 전달하는게 안전함)

```javascript
const result = [10, 20, 30, 40].reduce(
  (accumulator, currentValue, currentIndex, array) =>
    accumulator + currentValue,
  0
);

console.log(result);
```

`Array.prototype.find` : 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환함. 존재하지 않으면 undefined 반환함

```javascript
const users = [
  { id: 1, nickname: 'jin' },
  { id: 2, nickname: 'pon' },
  { id: 3, nickname: 'nan' },
];

const result = users.find((user) => user.nickname === 'pon');

console.log(result); // { id: 2, nickname: 'pon'},

// true인 첫 번째 요소를 반환함
```

filter는 true인 요소만 추출한 새로운 배열을 반환하고, find는 true인 첫 번째 요소만 반환하는 차이가 있다.
