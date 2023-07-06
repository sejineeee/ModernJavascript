# async ... await

ES8에서는 제너레이터보다 간단하고 가독성 좋게 비동기처리를 동기처럼 동작하도록 구현할 수 있는 `async ... await`가 도입되었다.

## generator

제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수이다. `function*` 키워드로 선언하고, 하나 이상의 `yield` 표현식을 포함한다.

```javascript
function* generator() {
  yield 1;
  yield 2;
}

const generator2 = function* () {
  yield 3;
};

const obj = {
  * generator3() {
    yield 4;
  }
}

class MyClass {
  * generator4() {
    yield 5;
  }
}

const returnGenerator = generator();
```

- 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.

  - `yield` 키워드는 제너레이터 함수를 일시 중지했다가 재개할 수 있다.

  - 제너레이터 객체의 `next` 메서드를 호출하면 `yield` 표현식까지 실행되고 일시 중지되는데, 이때 함수의 제어권이 호출자로 양도됨.
  ```javascript
  function* generator() {
  yield 1;
  yield 2;
    }

    const returnGenerator = generator();

    returnGenerator.next();
  ```


- 함수 호출자에게 상태를 전달할 수 있고, 함수 호출자로부터 상태를 전달받을 수 있다.

  - next 메서드와 yield를 이용하여 상태를 전달하고 받는 것이 가능하다.

  - 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.
  ```javascript
  function* generatorFunc() {
  const x = yield 1;
  
  const y = yield (x + 10);
  
  return x + y;
  }

  const generator = generatorFunc();

  generator.next(); // {value: 1, done: false}
  // x 변수에는 값이 할당되지 않은 상태로 그 다음 next 메서드에 전달한 인수가 할당됨.
  generator.next(20); // {value: 30, done: false}
  // x 변수에 값이 20이 할당이 되었고, yield 표현식(x + 10)이 계산되어 value가 30이 됨.
  // y 변수에는 값이 할당되지 않은 상태로 그 다음 next 메서드에서 전달한 인수가 변수에 할당됨.
  generator.next(30); // {value: 50, done: true}
  // x 변수에는 20이 할당되고, y 변수에는 30이 할당됨. return 구문에 x + y 값이 value 프로퍼티에 할당됨.
  ```

### 문제

- generator를 이해해보기 위해서 직접 사용해보며 [문제](https://ko.javascript.info/generators)를 풀어보고자 했다.

```javascript
function* pseudoRandom(value) {
  const staticValue = value;

  let previous = yield staticValue * 16807 % 2147483647
  
}

let generator = pseudoRandom(1);
console.log(generator.next().value); // 16807

console.log(generator.next().value); // undefined
```

혼자 힘으로는 여기까지밖에 생각이 나지 않았다. 이게 아니란 걸 알면서도... 더 이상 생각나지 않았다. 저 previous 변수에는 next 메서드에 전달한 인수 값이 들어갈 것을 알면서...🥺 yield를 하고, 계산된 값을 previous에 할당해서 다시 반복해서 yield하면 얼마나 좋을까. 그러면 반복문을 사용해야 되겠지라는 생각은 들지만, 어떤 식으로 반복문 코드를 짜야 될까...

해답에서는 while 반복문을 이용하였다.

```javascript
function* pseudoRandom(number) {
  const staticValue = number;
  let value = staticValue;
  
  while(value) {
    value = value * 16807 % 2147483647;
    yield value;
  }
}

let generator = pseudoRandom(1);
console.log(generator.next().value);

console.log(generator.next().value);
```

## async ... await

`async` 키워드를 이용하여 함수를 정의하면 프로미스 값을 반환한다. `await` 키워드는 `await` 함수 내부에서 사용할 수 있고, 비동기 처리가 수행된 상태가 될 때까지 기다렸다가 프로미스가 resolve한 처리 결과를 반환한다.

```javascript
async function fetchData() {
    const url = 'https://eatgo-customer-api.ahastudio.com/categories';
    const response = await fetch(url);
    // response에는 HTTP 요청에 대한 응답이 도착해서 fetch 함수가 settled 상태가 될 때까지 대기
    // settled 상태가 되면 resolve한 결과가 response 변수에 할당됨.
    const data = await response.json();
    // response의 결과를 가지고 json 메서드를 사용하는 것이기 때문에 await 키워드를 이용하여 순차적으로 처리해야 된다.
  
    console.log(data); 
};
```

### try ... catch

```javascript
async function fetchData() {
    const url = 'https://eatgo-customer-api.ahastudio.com/categories';
  try {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);
  } catch(err) {
    console.error(err)  
  }
};
```
try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치할 수 있다.
async 함수 내에서 catch문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.

만약 async 함수 내에 catch 구문이 없다면 catch 메서드를 이용하면 된다.
```javascript

fetchData().catch(err => console.log(err));

fetchData().then(console.log).catch(console.error);
```

### Promise.all

여러 개의 프라미스가 모두 처리되길 기다려야 하는 상황이라면 `async/await` 구문을 `Promise.all`과 함께 쓸 수 있다.

`Promise.all()`은 프라미스인 배열을 받고, 새로운 프라미스를 반환한다. 전달되는 프라미스 중 하나라도 reject 된다면 반환하는 프라미스는 에러와 함께 rejected 됨.

```javascript
const categoryUrl = 'https://eatgo-customer-api.ahastudio.com/categories';

const regionUrl = 'https://eatgo-customer-api.ahastudio.com/regions';

let results = await Promise.all([
  fetch(categoryUrl),
  fetch(regionUrl),
]).then((response) => response.map((res) => res.json())).then((datas) => Promise.all(datas)).then((data) => data.map((item) => console.log(item)));
```

<img width="465" alt="스크린샷 2023-07-06 오후 2 46 37" src="https://github.com/sejineeee/ModernJavascript/assets/86041335/2d23c2ce-584a-4789-a1ad-db9fcec1b081">


#### 문제
모던자바스크립트 튜토리얼 홈페이지에 나와 있는 문제를 통해서 `async ... await`를 이해하고자 했다.

- async ... await으로 바꾸기
```javascript
function loadJson(url) {
  return fetch(url).then(response => {
    if(response.status === 200) {
      return response.json();
    } else {
      throw new Error(response.status);
    }
  })
}

loadJson('no-such-user.json')
  .catch(alert);
```

```javascript
const loadJson = async (url) => {
    try {
      const response = await fetch(url);
      const data = await response.json();

      return data;

    } catch(err) {
      alert(err);   
    }
};

const fetchData = await loadJson('no-such-user.json');
```

- ⭐️ async가 아닌 함수에서 async 함수 호출하기
```javascript
async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  return 10;
}

function f() {
  // ...코드...
  // async wait()를 호출하고 그 결과인 10을 얻을 때까지 기다리려면 어떻게 해야 할까요?
  // f는 일반 함수이기 때문에 여기선 'await'를 사용할 수 없다는 점에 주의하세요!
}
```

```javascript
async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));
  
  return 10;
}

function f() {
  return wait();
}

f().then(res => console.log(res));

function f1() {
  wait().then(res => console.log(res))
}

f1();
```

async가 아닌 함수에서 async 함수 호출하는 문제는 진짜 중요하다! 되게 쉬워보이지만, async에 대한 기본적인 것을 생각하지 않으면 풀 수 없다고 생각하기 때문에...😢

전에도 이러한 부분에 대해서 작성한 블로그가 있었다. [useEffect에서 async 키워드를 사용하면 생기는 일](https://velog.io/@sejinee/useEffect%EC%97%90%EC%84%9C-async-%ED%82%A4%EC%9B%8C%EB%93%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%96%88%EC%9D%84-%EB%95%8C-%EC%83%9D%EA%B8%B4-%EC%9D%BC%EB%93%A4)에 대해서 적었는데, 이때도 async가 아닌 함수에서 async를 호출하는 부분에 대해서 다뤘기 때문에 중요하다고 생각한다.

---

참고

- [모던 자바스크립트 튜토리얼](https://ko.javascript.info/async-await)