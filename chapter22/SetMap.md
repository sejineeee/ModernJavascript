# Set과 Map

## Set

set 객체는 중복되지 않는 유일한 값들의 집합으로 수학적 집합을 구현하기 위한 자료구조다.

```plainText
🤔 배열과 유사하지만 다른 점은?
- 중복 값을 포함할 수 없음
- 요소 순서에 의미가 없음
- 인덱스로 요소에 접근할 수 없음
```

Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성

```javascript
// 중복요소를 제거하고 반환함
const set = new Set([1, 2, 3, 4, 3, 2]);

console.log(set); // Set(4) { 1, 2, 3, 4 }

const set2 = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([])
  .add(() => {});

console.log(set2); // Set(8) { 1, 'a', true, undefined, null, Object, Array(0), () =>{}}

const { size } = set;
console.log(size); // 4
```

- `Set.prototype.size`를 이용하여 갯수를 확인할 수 있음

```javascript
const set = new Set([1, 2, 3, 4]);

set.add(5).add(0);
console.log(set);

set.add(NaN).add(NaN);
console.log(set);
// Set(7) { 1, 2, 3, 4, 5, 0, NaN }
```

- 요소를 추가할 때 `Set.prototype.add` 메서드 사용

- `add` 메서드는 추가된 Set 객체를 반환하기 때문에 메서드 체이닝이 가능함

- `NaN`을 일치연산자를 이용하여 비교했을 때는 `false` 값을 반환하지만, Set 객체는 `NaN`은 값이 같다고 생각하여 중복으로 추가되지 않음

```javascript
const set = new Set([1, 2, 3]);

console.log(set.has(3)); // true
console.log(set.delete(2)); // true
console.log(set.delete(4)); // false

console.log(set); // Set(2) {1, 3}

console.log(set.clear()); // undefined

console.log(set); // Set(0){}
```

- `Set.prototype.has` 메서드는 요소가 있을 경우 `true`를 반환하고, 없을 경우 `false` 반환함

- `Set.prototype.delete` 메서드를 이용해서 요소를 삭제할 수 있음. 요소가 존재해서 삭제 가능한 경우 `true` 값을 반환하고, 존재하지 않는 요소를 삭제하려고 한 경우 `false` 값을 반환함

- `Set.prototype.clear` 메서드를 이용해서 모든 요소를 삭제할 수 있음. `undefined` 값을 반환함

```javascript
const set = new Set([1, 2, 3, 4]);

set.forEach((number) => {
  console.log(number ** 2);
});

for (const number of set) {
  console.log(number ** 2);
}
```

- `Set.prototype.forEach` 메서드를 사용하여 Set 객체를 순회할 수 있음

- `for ... of`문을 사용해서도 Set 객체를 순회할 수 있음

### 교집합

```javascript
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    if (this.has(value)) result.add(value);
  }
  return result;
};

// 참고로 arrow function과 익명함수를 할당하는 것에서 this의 차이점이 있기 때문에 고려해야 된다

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 3]);

console.log(setA.intersection(setB));

// 이렇게도 쓸 수 있다
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((number) => set.has(number)));
};
```

```javascript
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 3]);

const intersection = new Set([...setA].filter((number) => setB.has(number)));

console.log(intersection);
```

### 합집합

```javascript
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([4, 5]);

Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

console.log(setA.union(setB));

// 이렇게도 만들 수 있음
Set.prototype.union = function (set) {
  const result = new Set(this);

  for (const number of set) {
    result.add(number);
  }
  return result;
};
```

### 차집합

```javascript
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 3]);

Set.prototype.difference = function (set) {
  const result = new Set(this);

  for (const number of set) {
    result.delete(number);
  }
  return result;
};

console.log(setA.difference(setB));

// 이렇게도 쓸 수 있다
Set.prototype.difference = function (set) {
  return new Set([...this].filter((value) => !set.has(value)));
};
// filter 메서드는 true를 반환하면 요소를 유지하고, false를 반환하면 버리기 때문에 차집합에서는 Not(!) 연산자를 사용함
```

## Map

Map 객체는 키와 값의 쌍으로 이루어진 컬렉션

```plainText
🤔 객체와의 차이점은?
- 키로 객체를 포함한 모든 값을 사용할 수 있음
- 이터러블함
```

map 생성자 함수는 `키와 값의 쌍으로 이루어진 이터러블을 인수로 전달받아` Map 객체를 생성함

```javascript
const map = new Map([
  ['key1', 'value1'],
  ['key2', 'value2'],
]);
console.log(map); // Map(2) { 'key1' => 'value1', 'key2' => 'value2' }

// key 값이 중복되면 값이 덮어씌워진다
const map2 = new Map([
  ['key1', 'value1'],
  ['key1', 'value2'],
]);
console.log(map2); // Map(1) { 'key1' => 'value2' }

console.log(map.size); // 2
```

- `Map.prototype.size`를 이용하여 갯수를 확인할 수 있음

```javascript
const map = new Map();

map.set({ name: 'Lee' }, 'user').set({ name: 'Park' }, 'admin');

console.log(map); // Map(2) { { name: 'Lee' } => 'user', { name: 'Park'} => 'admin' }

const map2 = new Map();

map2.set(NaN, 'value1').set(NaN, 'value2');

console.log(map2);
```

- 요소를 추가할 때 `Map.prototype.set` 메서드를 사용

- `set`메서드는 추가된 Map 객체를 반환하기 때문에 메서드 체이닝이 가능함

- `NaN`을 일치연산자를 이용하여 비교했을 때는 false 값을 반환하지만, Map 객체는 NaN은 값이 같다고 생각하여 중복으로 추가되지 않음

```javascript
const map = new Map([
  ['key1', 'value1'],
  ['key2', 'value2'],
]);

console.log(map.get('key1')); // 'value1'
console.log(map.has('key1')); // true
console.log(map.delete('key1')); // true
console.log(map); // Map(1) { 'key2' => 'value2' }
console.log(map.clear()); // undefined
console.log(map); // Map(0) {}
```

- `Map.prototype.get` 메서드를 사용하여 요소에 접근 가능함

- `Map.prototype.has` 메서드는 요소가 있을 경우 `true`를 반환하고, 없을 경우 `false` 반환함

- `Map.prototype.delete` 메서드를 이용해서 요소를 삭제할 수 있음. 요소가 존재해서 삭제 가능한 경우 `true` 값을 반환하고, 존재하지 않는 요소를 삭제하려고 한 경우 `false` 값을 반환함

- `Map.prototype.clear` 메서드를 이용해서 모든 요소를 삭제할 수 있음. `undefined` 값을 반환함

```javascript
const map = new Map([
  [{ name: 'sejin' }, 'user'],
  [{ name: 'jin' }, 'admin'],
]);

// forEach로 순회
map.forEach((value, key) => {
  console.log(value);
  console.log(key);
});

// for...of로 순회 및 Map.prototype.keys, Map.prototype.values, Map.prototype.entries
for (const key of map.keys()) {
  console.log(key);
}

for (const value of map.values()) {
  console.log(value);
}

for (const entry of map.entries()) {
  console.log(entry);
}
```

- `forEach` 이용하여 순회 가능
- `for ... of`문 이용하여 순회 가능
- `Map.prototype.keys, Map.prototype.values, Map.prototype.entries` 메서드 사용 가능함
