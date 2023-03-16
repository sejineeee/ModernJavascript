# 단락 평가

### 논리 연산자를 이용하여 단축평가

And 연산자를 이용하여 단축평가할 경우, 첫 번째 피연산자가 true인 경우 두 번째 피연산자가 반환되고, 첫 번째 피연산자가 false인 경우 첫 번째 피연산자가 반환된다.
```javascript
'Cat' && 'Dog'; // 'Dog'
false && 'Dog'; // false
```

OR 연산자를 이용하여 단축평가할 경우, 첫 번째 피연산자가 true인 경우 첫 번째 피연산자가 반환되고, 첫 번째 피연산자가 false인 경우 두 번째 피연산자가 반환된다.

```javascript
'Cat' || 'Dog'; // 'Cat'
false || 'Dog'; // 'Dog'
```

객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 사용한다.
```javascript
let elem = null;
// elem.value를 할 경우 null property를 읽을 수 없다고 한다. && 연산자를 사용
let value = elem && elem.value; // null
```

### 옵셔널 체이닝
ES11(ECMAScript2020)에서 옵셔널 체이닝 연산자가 도입되었다. 옵셔널 체이닝은 `?.` 사용하여 좌항의 피연산자가 `null` 또는 `undefined`인 경우 에러 대신에 `undefined`를 반환한다.

```javascript
let elem = null;

let value = elem?.value;
console.log(value); // undefined
```

옵셔널 체이닝이 도입되기 이전에는 `&&` 연산자를 사용하였다. 옵셔널 체이닝과 `&&`의 차이점이 있다. 옵셔널 체이닝은 null, undefined인 경우에만 undefined를 반환한다. 그렇기 때문에 falsy 값일 경우 `&&` 연산자는 falsy 값을 반환하지만, 옵셔널 체이닝 연산자는 falsy 값 중 null과 undefined가 아니면 우항의 프로퍼티를 참조한다.

### nullish coalescing 연산자

nullish coalescing 연산자는 `??` 기호를 사용한다.
`leftExpr ?? rightExpr`

왼쪽 피연산자가 null 또는 undefined일 때 오른쪽 피연산자를 반환하고, 그 외에는 왼쪽 피연산자를 반환한다.

```javascript
let result1;
let result2 = result1 ?? 100;

console.log(result2);

let result3 = 10;
let result4 = result3 ?? 100;
console.log(result4);

let result5 = null;
let result6 = result5 ?? 100;
console.log(result6);
```

nullish coalescing 연산자 이전에는 `||` 연산자를 이용해 단축 평가를 했다. nullish coalescing과 `||`의 차이점이 있다. falsy 값 중 null과 undefined가 아닌 경우 nullish 연산자는 좌항의 피연산자를 반환한다. `||` 연산자의 경우 falsy 값인 경우에 우항의 피연산자를 반환한다.
```javascript

const count = 0;

const result = count || 42;
console.log(result); // count가 falsy 값이기 때문에 우항 값인 42를 반환

const result2 = count ?? 42;
console.log(result2); // count가 null 또는 undefined가 아니기 때문에 count 값인 0을 반환한다.
```