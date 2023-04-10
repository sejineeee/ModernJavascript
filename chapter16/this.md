# this

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

함수가 호출되는 방식에 따라 this 바인딩이 동적으로 결정된다.

### 일반 함수 호출

일반 함수(중첩함수, 콜백함수 포함)로 호출하면 this에는 전역 객체가 바인딩 됨

```javascript
const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(`foo's value is ${this.value}`);
    
    function bar() {
      console.log(`bar's value is ${this.value}`);
    }
    
    bar();
  }
};

obj.foo();
// "foo's value is 100"
// "bar's value is undefined"
// undefined가 나오는 이유는 const 키워드를 이용해서 value를 선언했기 때문임
```

### 메서드 호출

메서드를 호출할 때, this는 메서드를 호출한 객체에 바인딩 됨

`obj.메서드()` 이렇게 있으면 `.` 앞에 객체에 바인딩

```javascript
const person = {
  name: 'sejin',
  getName() {
    return this.name;
  }
};

console.log(person.getName()); // 'sejin'
```

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성할 인스턴스가 바인딩 됨

```javascript
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  }
}

const circle1 = new Circle(5); // 인스턴스
const circle2 = new Circle(8); // 인스턴스

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 16
```

### Function.prototype.apply / call / bind 메서드에 의한 간접 호출

#### apply

`Function.prototype.apply(thisArg,[argsArray])`

메서드의 첫 번째 인자를 this로 바인딩하고, 두 번째 인자를 배열로 받아서 배열의 요소들을 호출할 함수의 매개변수로 지정함

#### call

`Function.prototype.call(thisArg, arg1...argN)`

메서드의 첫 번째 인자를 this로 바인딩하고, 이후의 인자들을 호출할 함수의 매개변수로 지정함

```javascript
function getThisBinding() {
  return this;
}


const person = {
  name: 'seijn',
}

getThisBinding.apply(person, [1, 2, 3]);
// { name: 'sejin' }
// arguments 1, 2, 3

getThisBinding.call(person, 1, 2);
// { name: 'sejin' }
// arguments 1, 2
```

#### bind

`Function.prototype.bind(thisArg, arg1...argN)`

메서드가 호출될 때, 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해서 반환함

이 메서드를 사용하면, 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결함

```javascript
const person = {
  name: 'sejin',
  foo(callback) {
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function() {
  console.log(`hi ${this.name}`);
}) // 'hi sejin'
```

---
참고

- [코어 자바스크립트 책](https://wikibook.co.kr/corejs/)