# 생성자 함수

```javascript
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

const circle1 = new Circle(4);
```
new 연산자와 함께 호출해야 생성자 함수로서 동작한다.


### 생성자 함수의 인스턴스 생성 과정

1. 암묵적으로 인스턴스가 생성되고, this에 바인딩된다.

2. this에 바인딩되어 있는 인스턴스를 초기화한다.

3. 완성된 인스턴스가 얌묵적으로 바인딩된 this를 반환한다.

this가 아닌 다른 객체([], {} 등)를 명시적으로 반환할 경우 this가 반환되지 않고 return문에 명시한 객체가 반환된다. 객체가 아닌 원시값인 경우에는 원시값은 무시되고 this가 반환됨

  ```javascript
  function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
        return 'sejin';
    }

    const circle1 = new Circle(4);
    console.log(circle1);
    /*
    Circle {
    radius: 4,
    getDiameter: ƒ (),
    __proto__: { constructor: ƒ Circle() }
    }
    */
  ```
  ```javascript
  function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
        return {};
    }

    const circle1 = new Circle(4);
    console.log(circle1); // {} 반환됨
  ```


### constructor & non-constructor

함수 객체는 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있는데, [[Call]]을 갖는 함수 객체를 callable, [[Construct]]를 갖는 함수 객체를 constructor라고 한다.

화살표 함수와 메서드는 non-constructor이다.
(⭐️ ES6 메서드 축약 표현)

```javascript
function bar() {
    console.log('constructor');
}

new bar(); // bar {}

const foo = function() {
    console.log('constructor')
}

new foo(); // foo {}

const arrowFn = () => {
    console.log('non-constructor');
};

new arrowFn(); // TypeError: arrowFn is not a constructor

const obj = {
  x: 1,
  getResult() {
    console.log('non-constructor');
  },
  setResult: function() {
    console.log('constructor');
  }
}

new obj.setResult(); // setResult {}

new obj.getResult(); // TypeError: obj.getResult is not a constructor
```

`new` 연산자와 함께 constructor를 호출해야 된다. 그래야 [[Construct]]가 호출된다. `new` 연산자 없이 호출할 경우 [[Call]]이 호출됨

`new.target` 메서드를 이용하여 `new` 연산자와 함께 호출이 되었는지 알 수 있다. `new` 연산자를 사용해서 호출됐을 경우, 함수 자신을 가리키고, 사용하지 않았을 경우 undefined 값을 반환함

```javascript
function Circle(radius) {
  if(!new.target) {
    return new Circle(radius);
    };
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
  }
}

const withoutNew = Circle(3);
```
---

`class`를 사용하면 `new` 연산자 없이 썼을 경우 에러가 발생하기 때문에 `new.target`을 확인하는 과정이 없어도 된다.
```javascript
class Circle {
  constructor(radius) {
    this.radius = radius;  
  }
  getDiameter() {
    return 2 * this.radius
  }
}

const circle = new Circle(4);
console.log(circle.getDiameter()); // 8

const withoutNew = Circle(4);
// TypeError: Class constructor Circle cannot be invoked without 'new'
```