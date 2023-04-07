# 프로토타입

javascript의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있고, Prototype 객체(부모 객체)의 프로퍼티 또는 메소드를 상속 받아서 사용할 수 있다. 프로토타입을 기반으로 상속을 구현한다.
<br />
<br />

![prototype-img](https://user-images.githubusercontent.com/86041335/230548547-b378690a-7fab-4fa8-8140-a1c5be774c2e.png)

코어 자바스크립트 책에서는 프로토타입을 도식화해서 설명하여, 조금 더 쉽게 이해할 수 있었다. ([도식화 이미지 출처](https://blog.makerjun.com/d79d8db4-2c47-4a5d-9fb8-74af75199c16))


생성자 함수(Constructor)를 new 연산자와 함께 호출하면 instance가 생성되고, instance에는 자동으로 `__proto__`가 부여되고, 이 프로퍼티는 Constructor의 prototype 프로퍼티를 참조한다. `__proto__`는 생략이 가능하기 때문에 생성자 함수의 prototype에 메서드나 프로퍼티에 접근할 때, `instance.메서드` 이렇게 자신의 것처럼 접근할 수 있다.

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('sejin');

console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

### constructor 프로퍼티

모든 프로토타입은 constructor 프로퍼티를 가진다. constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴
```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('sejin');

console.log(Person.prototype.constructor === Person); // true
console.log(me.constructor === Person); // true
```

### 프로토타입 체인

자바스크립트는 객체의 프로퍼티에 접근하려고 할 때, 해당 객체에 접근하려는 프로퍼티가 없을 경우, `[[Prototype]]` 내부 슬롯의 참조를 따라 프로토타입의 프로퍼티를 순차적으로 검색하는 것을 프로토타입 체이닝이라고 한다.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function() {
  console.log(`hello ${this.name}`);
};

const me = new Person('sejin');

me.sayHello();
// me(.__proto__).sayHello();

me.hasOwnProperty('name');
// me(.__proto__)(.__proto__).hasOwnProperty('name')
```
`__proto__`는 생략 가능하기 때문에 프로토타입의 메서드를 자신의 메서드인 것처럼 실행할 수 있는 것이다.

### 오버라이딩 & property shadowing


```javascript
const Person = function(name) {
  this.name = name;
};

Person.prototype.getName = function() {
  console.log(this.name);
}

const me = new Person('sejin');
me.getName = function() {
  console.log(`hi, ${this.name}`);
};

me.getName();
```
프로토타입의 프로퍼티(메서드)와 같은 이름의 프로퍼티를 인스턴스에 추가할 경우 메서드 오버라이드 현상이 일어난다. 이렇게 상속으로 프로퍼티가 가려지는 현상을 property shadowing이라고 한다.

이런 경우, 프로토타입 프로퍼티가 없어진 것은 아니고, 원본은 그대로 있는 상태로 추가된 것을 그 위에 얹는다고 생각하자...! 그러고 프로퍼티에 접근할 때는 가장 가까운 자신의 프로퍼티를 검색하고, 없으면 위로, 위로, 이렇게 올라가는 프로토타입 체이닝을 생각하면 됨...


### 정적 메서드와 프로토타입 메서드

```javascript
function Foo() {}

// 프로토타입 메서드
Foo.prototype.sayHi = function() {
  console.log('hi');
};

const foo = new Foo();
foo.sayHi();

// 정적 메서드
Foo.sayHello = function() {
  console.log('hello');
};

Foo.sayHello();

```
<img width="297" alt="mdn 사이트 메서드 일부" src="https://user-images.githubusercontent.com/86041335/230634176-e6e42f27-7328-494b-b4e1-89f5b884fadf.png">

mdn 사이트에서도 Object의 여러 메서드를 볼 수 있는데, 여기서도 정적 메서드와 프로토타입 메서드를 구분할 수 있다. `Object.is` 같은 메서드는 정적 메서드, `Object.prototype.hasOwnProperty` 같은 메서드는 프로토타입 메서드이다.

---
참고
- [코어 자바스크립트 책](https://wikibook.co.kr/corejs/)
- [메이커준 블로그](https://blog.makerjun.com/d79d8db4-2c47-4a5d-9fb8-74af75199c16)