# 제어문
제어문은 코드의 실행흐름을 인위적으로 제어할 수 있으나 코드의 흐름을 이해가기 어렵게 만들어 가독성을 해친다.

## 블록문
블록문은 자체 종결성을 갖고 있기 때문에 블록문 뒤에는 세미콜론을 붙이지 않는다.

## 조건문

### if ... else

`if ... else`문이 중첩이 되고 복잡해질 경우 가독성을 해칠 수 있기 때문에 `Guard clauses`를 적용하여 리팩토링을 하면 좋다.

```javascript
let userList = [
  { id: 1, name: 'sejin1' },
  { id: 2, name: 'sejin2' },
  { id: 3, name: 'sejin3' },
]

const getUserList = (list) => {
  if (!list.length) {
    return 'user list가 없습니다.';
  }
  
  list.map((item) => {
    return console.log(item);
  });
}

getUserList(userList); // useList 배열 안에 있는 객체들이 콘솔에 출력

// userList가 빈 배열인 경우

userList = [];

getUserList(userList); // user list가 없습니다 반환
```
Guard 조건을 Not Guard로 바꿔 early return을 해준다.

### switch
```javascript
switch (표현식) {
    case 표현식 : 
        실행문;
        break;
    case 표현식 :
        실행문;
        break;
    default:
        실행문
}
```
fall through에 의해서 일치하는 case가 아니더라도 실행이 되기 때문에 break문을 써줘야 된다.

```javascript
const action = "say_hello";
switch (action) {
  case "say_hello":
    const message = "hello";
    console.log(message);
    break;
  case "say_hi":
    const message = "hi";
    console.log(message);
    break;
  default:
    console.log("Empty action received.");
}
```
이렇게 쓰면 렉시컬 스코프에 의해 변수 message가 이미 선언이 되었기 때문에 재선언할 수 없다고 syntax error가 발생한다. 그렇기 때문에 case 실행문을 `{}`으로 감싸줘야 된다.

## 반복문

조건식의 결과가 참인 경우 코드 블록을 실행하고, 거짓이 될 때까지 반복한다.

### for
반복 횟수가 명확할 때 사용하고, for문을 중첩할 수 있다.
```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}
// 0, 1, 2
```
변수 선언문, 조건식, 증감식은 옵션이기 때문에 선언하지 않아도 되지만, 아무것도 선언하지 않으면 무한루프가 되기 때문에 주의해야 된다.

```javascript
let i = 0;
for(; i < 3; i++) {
    console.log(i);
} // 변수 선언을 전역에서 선언함
```
```javascript
for (let i = 0; ; i++) {
    console.log(i)
    if(i > 3) {
        break;
    }
}
```
```javascript
let i = 0;

for (;;) {
  if (i > 3) break;
  console.log(i);
  i++;
}
```


### while
반복 횟수가 불명확할 때 주로 사용한다.

```javascript
let i = 0;

while(i <= 3) {
    console.log(i);
    i++
}
```

### do ... while
코드 블록 먼저 실행하고 조건식을 평가하기 때문에 코드블록은 무조건 한번 이상 실행된다.

```javascript
let i = 0;
do {
    console.log(i);

} while(i < 4);

do {
    console.log(i);
    i++
} while(i < -1);
// 무조건 한번은 실행되기 때문에 0이 출력된다.
```

## break / continue

break문은 레이블문, 반복문, switch문의 코드 블록을 탈출한다.

```plaintext
❓레이블문
레이블 구문은 식별자가 붙은 문을 말한다.
label: statement

레이블문은 프로그램의 흐름이 복잡해져서 가독성을 해치고, 오류를 발생시킬 가능성이 높아진다.
```
```javascript
foo: {
    console.log(0);
    break foo;
    console.log(1);
} // foo 레이블 블록문을 탈출했기 때문에 0만 실행된다.
```

continue문은 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시켜 다시 실행한다.
```javascript
let string = 'hello';
let search = 'e';
let count = 0;


for (let i = 0; i < string.length; i++) {
  if(string[i] === search) {
    continue;
  }
  count++
  console.log(count);
} // 1 2 3 4 출력
```
---
참고

[Guard clauses](https://techbless.github.io/2021/06/16/%EC%A4%91%EC%B2%A9-%EC%A1%B0%EA%B1%B4%EB%AC%B8%EC%9D%84-Guard-Clause%EB%A1%9C-%EA%B0%80%EB%8F%85%EC%84%B1-%EB%86%92%EC%9D%80-%EC%BD%94%EB%93%9C%EB%A5%BC-%EC%9E%91%EC%84%B1%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95/)

[switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch#breaking_and_fall-through)

[label](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/label)