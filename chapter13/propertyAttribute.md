# 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드
자바스크립트에는 내부 슬롯과 내부 메서드가 존재하지만, 내부 로직이기 때문에 직접적으로 접근하거나 호출할 수 없다.

일부 내부 슬롯과 내부 메서드에 한하여 접근할 수 있다. 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는데, `Object.getPrototypeOf(obj)` 메서드를 이용해서 간접적으로 접근할 수 있다.

## 프로퍼티 어트리뷰트

자바스크립트 엔진은 프로퍼티를 생성할 때, 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의함

`Object.getOwnPropertyDescriptor()` 메서드를 사용하여 프로퍼티 어트리뷰트를 간접적으로 확인할 수 있다.

```javascript
const person = {
  name: 'park',
};

console.log(Object.getOwnPropertyDescriptor(person, 'name'))

/*
{
  value: 'park', -> [[Value]]
  writable: true, -> [[Writable]]
  enumerable: true, -> [[Enumerable]]
  configurable: true -> [[Configurable]]
}
*/
```

### 데이터 프로퍼티와 접근자 프로퍼티

#### 데이터 프로퍼티
키와 값으로 구성된 일반적인 프로퍼티

- `[[Value]]` : 값
- `[[Writable]]` : 프로퍼티 값의 변경 가능여부로 boolean 값을 가짐. false인 경우 읽기 전용 프로퍼티가 됨
- `[[Enumerable]]` : 열거 가능 여부로 boolean 값을 가짐. 
- `[[Configurable]]` : 재정의 가능 여부로 boolean 값을 가짐.

#### 접근자 프로퍼티
자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티

- `[[Get]]` : 접근자 프로퍼티로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 `[[Get]]`의 값인 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환

- `[[Set]]` : 접근자 프로퍼티로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 `[[Set]]`의 값인 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장
- `[[Enumerable]]`
- `[[Configurable]]`

```javascript

const user = {
  firstName: 'sejin',
  lastName: 'Park',
  
  get fullName() {
  return `${this.firstName} ${this.lastName}`;
},
  
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

console.log(Object.getOwnPropertyDescriptor(user, 'fullName'));

/*
{
  get: ƒ get fullName(),
  set: ƒ set fullName(),
  enumerable: true,
  configurable: true
}
*/
```

### 프로퍼티 정의

```javascript
const user = {};

Object.defineProperty(user, 'name', {
  value: 'sejin',
  writable: true,
});
// descriptor 객체에 enumerable, configurable 속성이 없음. 이런 경우에는 false 값이 기본임. value 속성만 디폴트 값이 undefined임

console.log(Object.getOwnPropertyDescriptor(user, 'name'));

/*
{
  value: 'sejin',
  writable: true,
  enumerable: false,
  configurable: false
}
*/

console.log(user.name); // 'sejin'

user.name = '세진';
// writable에 true 값을 주었기 때문에 값을 변경할 수 있음

console.log(Object.getOwnPropertyDescriptor(user, 'name'));
/*
{
  value: '세진',
  writable: true,
  enumerable: false,
  configurable: false
}
*/

// writable 속성을 false로 줄 경우
// 값을 변경하려고 하면 값이 변경되지 않는다.
// 에러가 발생하지는 않음
```

`Object.defineProperties`를 이용해서 여러 프로퍼티를 한번에 정의
```javascript
const user = {};

Object.defineProperties(user, {
  firstName: {
    value: 'sejin',
    writable: true,
  },
  lastName: {
    value: 'Park',
    writable: true,
  },
  
  fullName: {
    get() {
      return `${this.firstName}, ${this.lastName}`
    },
    
    set(name) {
      [this.firstName, this.lastName ] = name.split(' ');
    },
    enumerable: true,
    configurable: true,
  }
});


user.fullName = 'sejin Park';

console.log(user.fullName);
```

## 객체 변경 방지

얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다.

### 객체 확장 금지
프로퍼티 추가를 금지한다.
`Object.preventExtensions` 메서드를 이용하면 프로퍼티 추가를 금지할 수 있다. (삭제는 할 수 있음)

```javascript
// 객체 확장 금지를 안 할 경우
const obj = {
  name: 'sejin',
};

obj.age = 29;

console.log(obj); // { name: 'sejin', age: 29 }

console.log(Object.isExtensible(obj)); // true

// 객체 확장 금지할 경우
const obj = {
  name: 'sejin',
};

Object.preventExtensions(obj);

obj.age = 29; // 에러가 발생하지는 않음

console.log(obj); // { name: 'sejin' }

console.log(Object.isExtensible(obj)); // false
```

### 객체 밀봉

프로퍼티 추가, 삭제, 어트리뷰트 재정의를 금지한다.
`Object.seal` 메서드를 이용하면 프로퍼티 추가, 삭제, 어트리뷰트 재정의가 금지된다. 프로퍼티에 값을 변경하는 것은 가능함

```javascript
const user = { name: 'sejin' };

Object.seal(user);

user.name = '세진';
console.log(user); // {name: '세진'}

console.log(Object.getOwnPropertyDescriptor(user, 'name'));
/*
{
  value: '세진',
  writable: true,
  enumerable: true,
  configurable: false
}
*/
```

### 객체 동결

`Object.freeze` 메서드를 이용하면 프로퍼티 추가 및 삭제, 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지 된다.

```javascript
const user = { name: 'sejin' };

Object.freeze(user);

user.name = '세진';

console.log(Object.getOwnPropertyDescriptor(user, 'name'));
/*
{
  value: 'sejin',
  writable: false,
  enumerable: true,
  configurable: false
}
*/
// writable, configurable 값이 false로 바뀜
```

`Object.freeze()`를 보니 생각난 게 하나 있다.
타입스크립트를 공부하면서 본 Utility Types 중 `Readonly<Type>`이 생각난다.
```typescript
interface User {
    name: string;
}

const user: Readonly<User> = {
    name: 'sejin',
}
```
[typescrip playground](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgKoGdrIN4ChkHIhwC2EAXMumFKAOYDcuAvrrggPYjXICumUSgCUIcACZcANgE8APBmgA+ZAF4c+QsTKUA5JgBWoHQBoWbftAB0WlGp2AOQcAjkzoZA)
에서 직접 실행해봄

---
참고

[Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
