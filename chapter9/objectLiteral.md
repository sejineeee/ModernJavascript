# 객체 리터럴

## 객체
객체는 0개 이상의 프로퍼티로 구성된 집합이다.
```javascript
let user = {
    name: 'sejin', // key: value  -> 프로퍼티라고 함
};
```

### 객체 생성 방법
자바스크립트는 프로토타입 기반의 객체지향 언어로서 다양한 객체 생성 방법을 지원한다.
```javascript
let a = {}; // 객체 리터럴

let b = Object(); // Object 생성자 함수

// 생성자 함수
function User(name) {
    this.name = name;
}

let c = new User('sejin');
console.log(c.name); // 'sejin'


// Object.create 메서드
let user = {
    isUser: true
}; 

let d = Object.create(user);
console.log(d.isUser); // true

// class
class User {
  constructor(name) {
    this.name = name;
  }
} 

let e = new User('sejin');
console.log(e.name); // 'seijn'
```

## 프로퍼티
```javascript
let person = {
    firstName: 'sejin',
}

let number = {
    '0': 0,
    '1': 1,
}
```
식별자 네이밍 규칙을 따르지 않는 키는 따옴표를 사용해서 나타내야 된다. 그에 따라 프로퍼티 키에 접근하는 방법도 달라진다.


```javascript
let x = 1;
let y = 2;

const obj = {
  x, // x: x,
  y, // y: y,
};

console.log(obj.x);
```
프로퍼티의 키와 값이 똑같을 때, 키를 생략할 수 있다.

### 프로퍼티 접근 방법

#### dot-notation
```javascript
let user = {
    id: 'sejin',
    password: 'a123',
    '0': 0,
};

console.log(user.id); // 'sejin'
console.log(user.0); // 에러 발생함. bracket notation으로 접근해야 됨
```

```javascript
let user = {
    firstName: 'sejin',
    'last-name': 'Park',
}

console.log(user.last-name); // NaN
```
이 경우 NaN이 나오는 이유는 `user.last`는 undefined 값, name은 전역변수 window.name으로 인식되어 `undefined - ''`으로 NaN이 나오는 것이다.

#### bracket
```javascript
let number = {
    '0': 0,
    '1': 1,
}

console.log(number[0]); // '0' 이렇게 접근하지 않고, 0 이렇게 접근할 수 있다.
```

### 프로퍼티 삭제
`delete` 연산자를 사용해서 객체의 프로퍼티를 삭제할 수 있다. 존재하지 않는 프로퍼티여도 에러가 발생하지 않는다.
```javascript
let user = {
    firstName: 'sejin',
    'last-name': 'Park',
}

delete person.firstName;
delete person['last-name'];
```

### 메서드 축약 표현
ES6사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다. 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와는 다르게 동작한다.

```javascript
const user = {
    name: 'sejin',
    sayHello() {
        console.log('hello');
    },
    sayHi: function() {
        console.log(`Hi ${this.name}`)
    }
};

let a = new user.sayHello(); // TypeError: user.sayHello is not a constructor
let b = new user.sayHi();

console.log(user.sayHello.hasOwnProperty('prototype')); // false

console.log(user.sayHi.hasOwnProperty('prototype')); // true
```
메서드는 인스턴스를 생성할 수 없기 때문에 생성자 함수로서 호출할 수 없다. 메서드는 프로퍼티가 없고 프로토타입도 생성하지 않는다.

---
참고
- [모던 JavaScript 튜토리얼](https://ko.javascript.info/)
- mdn 사이트