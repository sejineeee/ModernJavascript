# 클래스

## 클래스와 생성자 함수의 차이

|          ㅤ           |                                 클래스                                  |        생성자함수         |
| :-------------------: | :---------------------------------------------------------------------: | :-----------------------: |
|      new 연산자       |                                  필수                                   |  없으면 일반 함수로 호출  |
| extends, super 키워드 |                                   ⭕️                                   |            ❌             |
|       호이스팅        |                호이스팅이 발생하지 않는 것처럼 보임(TDZ)                |            ⭕️            |
|      strict mode      |             암묵적으로 strict mode 지정되어 해제할 수 없음              | strict mode 지정되지 않음 |
|      Enumerable       | constructor, 프로토타입 메서드, 정적 메서드는 [[Enumerable]] 값이 false |            ㅤ             |

## 클래스

- 클래스는 `class` 키워드를 사용하여 정의하고, `PascalCase`를 사용하여 네이밍한다.

- 클래스는 함수로, 값처럼 사용할 수 있는 일급 객체임
  ```javascript
  const Person = class {};
  // 표현식으로 정의할 수 있다는 것은 일급 객체라는 것 의미
  ```

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`hi~ ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log("hello");
  }
}

const me = new Person("seijn");
me.sayHi();
Person.sayHello();
```

### constructor

constructor는 인스턴스를 생성하고 초기화하기 위한 생성자 함수이다. class 내에 최대 1개만 존재할 수 있고, 생략 가능하다.

constructor 내부에서 return문을 사용하면 안된다. return 문을 사용하면 인스턴스를 반환하지 못하고, return문에 명시한 객체가 반환되기 때문에 사용하면 안된다.

```javascript
class Person {
  constructor(name) {
    // 인수로 인스턴스 초기화
    this.name = name;
  }
}

// return문 사용 ❌
class Person2 {
  constructor(name) {
    // 인수로 인스턴스 초기화
    this.name = name;
    return {};
    // 인스턴스를 반환하지 못하고, {} 객체를 반환함
    // return 'hi'처럼 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 인스턴스(this)를 반환함
  }
}
```

### 프로토타입 메서드

생성자 함수에 의한 객체 생성 방식과 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨

### 정적 메서드

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다. 메서드에 `static` 키워드를 붙이면 정적 메서드가 된다. 정적 메서드는 클래스의 인스턴스에서 직접 호출할 수 없다. 클래스로 호출해야 된다.

정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용하다.

```javascript
// 표준 빌트인 객체인 Math, Number, Object, JSON 등은 다양한 정적 메서드를 갖고 있음

Math.random();
Number.isNan(NaN);
```

### 프로퍼티

#### class field

클래스 필드는 생성자의 인수에 의존할 수 없다. 빈 클래스 필드를 선언할 경우, 필드의 존재 여부를 나타낼 수 있으며, 타입 체커 뿐만 아니라 코드를 읽는 사용자가 클래스의 구조를 정적으로 분석할 수 있어 유용하다.

```javascript
class Person {
  // 빈 클래스 필드를 선언함
  // 초기값을 할당하지 않으면 undefined
  name;
  constructor(name) {
    this.name = name;
  }

  // 클래스 필드를 통해 메서드를 정의
  getName = function () {
    return this.name;
  };
}
```

클래스 필드는 인스턴스 프로퍼티가 되기 때문에 클래스 필드에 함수를 할당한 경우 인스턴스 메서드가 되기 때문에 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.

#### private field

`#` 키워드를 앞에 붙여주면 private field를 정의할 수 있고, 클래스 내에서 private field를 참조할 때도 `#`를 붙여줘야 된다.  
(private field는 constructor 내에서 정의하면 에러 발생함)

```javascript
class Person {
  #name = "init";
  constructor(name) {
    // private field를 참조할 때 #를 붙이면 접근 가능함
    this.#name = name;
  }
}

const me = new Person("sejin");
console.log(me.name); // undefined;
console.log(me.#name); // error
```

#### static field

`static` 키워드를 붙이면 클래스의 인스턴스에서 직접 접근할 수 없다. 클래스에서 접근이 가능하다.

```javascript
class Person {
  static name = "sejin";
}

console.log(Person.name);

const me = new Person();

console.log(me.name);
```

### 상속

`extends` 키워드를 사용하여 클래스를 상속을 통해 확장시켜 사용할 수 있다.

- 수퍼 클래스 : 부모 클래스
- 서브 클래스 : 상속받은 자식 클래스

```javascript
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

class withOut extends Base {}

class Derived extends Base {
  // constructor를 생략하지 않은 경우에는 서브 클래스의 constructor에 반드시 super를 호출해야 됨.
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}

const without = new WithOut("with", "out");
console.log(without.a); // 'with'

const derived = new Derived(1, 2, 3);
console.log(derived.a); // 1
console.log(derived.c); // 3
```

`super`키워드를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출하고, 참조하면 수퍼클래스의 메서드를 호출할 수 있다. (❗️화살표 함수는 super를 지원하지 않음)

**`super`를 호출할 경우**

- 서브 클래스에서 constructor를 생략하지 않은 경우 서브 클래스의 constructor에서는 반드시 super를 호출

- 서브 클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없음

- super는 반드시 서브 클래스의 constructor 안에서만 호출

** `super`를 참조할 경우**

- 서브 클래스의 프로토타입 메서드 내에서 super를 참조할 때는 super를 붙여주면 수퍼클래스의 프로토타입 메서드를 가리킨다.(ES6 메서드 축약 표현으로 정의된 함수만 가능함)

  ```javascript
  class Base {
    constructor(a, b) {
      this.a = a;
      this.b = b;
    }
    add() {
      return this.a + this.b;
    }
  }

  class Derived extends Base {
    constructor(a, b, c) {
      super(a, b);
      this.c = c;
    }
    add() {
      // super.add는 수퍼클래스의 프로토타입 메서드 add를 가리킴
      return super.add() + this.c;
    }
  }

  const derived = new Derived(1, 2, 3);
  derived.add();
  ```

- 서브 클래스의 정적 메서드 내에서 super를 붙여 참조하면 수퍼클래스의 정적 메서드를 가리킨다.

---

javascript에서는 private 필드를 구현할 때 `#` 키워드를 이용해야 됐고, private랑 public만 사용 가능했지만, typescript에서는 public, protected, private 키워드를 사용하여 속성의 접근을 지정할 수 있다.

```typescript
// private는 외부에서 접근 불가
// protected는 선언된 클래스의 서브 클래스에서만 접근할 수 있음
class Player {
  constructor(
    private firstName: string,
    protected lastName: string,
    public nickname: string
  ) {}
}

const me = new Player("sejin", "park", "세지니");
console.log(me.nickname);
console.log(me.firstName);

class Me extends Player {
  public sayLastName() {
    return this.lastName;
  }
}

const me2 = new Me("sejin", "park", "세지니");
console.log(me2.sayLastName());
```
