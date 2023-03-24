# 원시 값과 객체의 비교

## 원시 값

원시 값은 변경 불가능한 값으로 한번 생성된 원시 값은 읽기 전용 값이다. 원시 값을 할당한 변수에 새로운 원시 값을 재할당하면 기존에 있던 원시 값을 변경하는 것이 아닌, 새로운 메모리 공간을 확보하고 재할당한 원시 값을 저장한 후, 그 메모리 주소를 가리키는 것이다.

```javascript
let a = 3; // 3을 저장하고 있는 메모리 주소를 가리키고 있음.

a = 4; // 4를 재할당하면, 3을 저장하고 있는 메모리 주소를 가리키고 있다가, 4를 저장하고 있는 메모리 주소로 바꾼다.
```

상수는 재할당이 금지된 변수일 뿐이지, 변경 불가능한 값이 아니다.
```javascript
const a = 3;

a = 4;

// TypeError: Assignment to constant variable.
```

```javascript
let a = 80;
let b = a;

a = 100;
console.log(`a 값은 ${a}`); // 'a 값은 100'
console.log(`b 값은 ${b}`); // 'b 값은 80'
```
a 변수와 b 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다. 변수에는 값이 전달되는 것이 아니라 메모리 주소가 전달되기 때문이다.

결국은 두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없다는 것이다.

## 객체

객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 참조 값에 접근할 수 있다. 참조 값은 생성된 객체가 저장된 메모리 공간의 주소 그 자체다.

객체를 생성하고 관리하는 방식은 매우 복잡하며 비용이 많이 들기 때문에 효율적으로 사용하기 위해서 객체는 변경 가능한 값으로 설계되어 있다.

객체는 원시 값과 다르게 여러 개의 식별자가 하나의 객체를 공유할 수 있기 때문에 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받게 된다.

```javascript
const person = {
    name: 'park'
};

console.log(person); // { name: 'park' };

person.name = 'sejin';

console.log(person); // { name: 'sejin' };
```
            
### 얕은 복사 & 깊은 복사
얕은 복사는 한 단계까지만(바로 아래 단계의 값만) 복사하고, 깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사하는 것을 말한다.

```javascript
const o = {
  x: {
    y: 1,
  }
};

// spread operator는 얕은 복사여서 y 변수까지는 복사를 하지 못한다.

const c1 = { ...o };

console.log(c1 === o); // false
// 원본가 복사본은 참조값이 다른 별개의 객체임.

console.log(c1.x === o.x); // true
// 값으로 평가될 수 있는 표현식이기 때문에 값을 비교해서 true가 나옴


// 객체에 중첩되어 있는 객체에 접근해서 값을 재할당 할 경우 => 원본 객체에 있는 프로퍼티 값도 변경된다.

console.log(c1.x.y = '안녕');

console.log(c1); // { x: { y: '안녕' } }
console.log(o); // { x: { y: '안녕' } }
```

```javascript
const a = {
  x: 1,
};

const c2 = { ...a };
console.log(c2);

c2.x = '안녕하세요';

console.log(c2); // { x: '안녕하세요' }
console.log(a); // { x: 1 }

// one depth까지는 복사했기 때문에 복사된 객체의 값을 변경해도 원본 객체의 값은 변하지 않는다.
```

#### JSON 메서드를 이용한 깊은 복사
객체에 중첩된 객체를 복사하기 위해서 `JSON.stringify()` 메서드를 이용하여 문자열로 전환했다가, `JSON.parse()` 메서드를 이용해서 객체로 바꿔주면 된다.

```javascript
const copyObjectViaJSON = (target) => {
  return JSON.parse(JSON.stringify(target));
};

const obj = {
  a: 1,
  b: {
    c: 2,
    d: ['hi', 'hello'],
    func: function() {
      console.log(3);
    },
    func2: function() {
      console.log(4);
    }
  }
};

const obj2 = copyObjectViaJSON(obj);

console.log(obj2);
// { a: 1, b: { c: 2, d: [ 'hi', 'hello' ] } }
```
다만 메서드나 숨겨진 프로퍼티인 __proto__나 getter/setter 등과 같이 JSON으로 변경할 수 없는 프로퍼티들은 모두 무시하기 때문에 복사되지 않는다.


#### lodash를 이용하여 깊은 복사

[lodash cloneDeep](https://lodash.com/docs/4.17.15#cloneDeep)

`npm i --save lodash` 를 이용해서 lodash를 설치한 뒤 사용할 수 있다.
```javascript
const _ = require('lodash');

const user = {
    name: {
        realName: 'sejin',
        nickName: 'jinee',
    },
    level: 60,
    skill: {
        attack(user) {
            return `${user} 공격!`;
        }
    }
};

const copyUser = _.cloneDeep(user);
console.log(copyUser);
/*
{
  name: { realName: 'sejin', nickName: 'jinee' },
  level: 60,
  skill: { attack: [Function: attack] }
}
*/

console.log(copyUser.skill.attack('jin')); // jin 공격!

console.log(copyUser.name === user.name); // false
```
lodash를 이용해서 깊은 복사를 하면 JSON과 다르게 메서드를 사용할 수 있다.


---
참고
- 코어 자바스크립트 책
- [lodash](https://lodash.com/)