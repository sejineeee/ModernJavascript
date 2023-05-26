# 정규표현식

정규표현식은 2가지 방법으로 만들 수 있다.

- literal notation
- RegExp 생성자 함수

```javascript
const literal = /pattern/(flag);

const regexp = new RegExp(/is/i);
const regexp2 = new RegExp(/is/, 'i');
const regexp3 = new RegExp('is', 'i');
```

## 정규표현식 메서드

`RegExp.prototype.exec`
인수로 전달받은 문자열에 대해 정규표현식 패턴을 검색하여 매칭 결과를 배열로 반환함. 없으면 null 반환함

```javascript
const target = 'star STARBUCKS STAR, star twinkle star';

const regexp = /star/g;

regexp.exec(target); // [ 'star', index: 0, input: 'star STARBUCKS STAR, star twinkle star', groups: undefined ]

// g플래그를 지정해도 첫 번째 매칭결과만 반환함
```

`RegExp.prototype.test`
인수로 전달받은 문자열에 대해 정규표현식 패턴을 검색하여 매칭 결과를 불리언 값으로 반환함.

```javascript
const target = 'star STARBUCKS STAR, star twinkle star';

const regexp = /starbucks/i;

regexp.test(target); // true
```

`String.prototype.match`
인수로 전달받은 정규 표현식과의 매칭결과를 배열로 반환함.

```javascript
const target = 'star STARBUCKS STAR, star twinkle star';

const regexp = /star/g;

target.match(regexp); // [ 'star', 'star', 'star' ]
```

## 플래그

| 플래그 |    의미     |                    설명                    |
| :----: | :---------: | :----------------------------------------: |
|   g    |   global    |                 전역 검색                  |
|   i    | ignore case |          대소문자를 구별하지 않음          |
|   m    | multi line  | 문자열의 행이 바뀌어도 패턴 검색을 계속 함 |
|   u    |   unicode   |           유니코드 문자를 처리함           |

## 패턴

### 문자열 검색

```javascript
const target = 'Is this all there is';

const regexp = /is/i;

target.match(regexp); // [ 'Is', index: 0, input: 'Is this all there is', groups: undefined ]

const regexp2 = /is/gi;

target.match(regexp2); // [ 'Is', 'is', 'is' ]
// 플래그에 g를 추가하면 전체적으로 검색함
```

| 패턴  |                                의미                                 |
| :---: | :-----------------------------------------------------------------: |
|   .   |                            임의의 문자열                            |
| {m,n} | 최소 m번, 최대 n번 반복되는 문자열 검색( `,` 뒤에 공백 있으면 안됨) |
|  {n}  |                      n번 반복되는 문자열 검색                       |
| {n, } |                 최소 n번 이상 반복되는 문자열 검색                  |
|   +   |        앞선 패턴이 최소 한번 이상 반복되는 문자열 검색 {1,}         |
|   ?   |    앞선 패턴이 최대 한번(0 포함) 이상 반복되는 문자열 의미 {0,1}    |

```javascript
const target = 'A AA BB AB aA Ba C AAA';

const regexp = /.../gi;

target.match(regexp); // [ 'A A', 'A B', 'B A', 'B a', 'A B', 'a C', ' AA' ]

const regexp1 = /A{1,3}/gi;

target.match(regexp1); // [ 'A', 'AA', 'A', 'aA', 'a', 'AAA' ]

const regexp2 = /A{2}/gi;

target.match(regexp2); // [ 'AA', 'aA', 'AA' ]

const regexp3 = /A{2,}/gi;

target.match(regexp3); // [ 'AA', 'aA', 'AAA' ]

const regexp4 = /A+/gi;

target.match(regexp4); // [ 'A', 'AA', 'A', 'aA', 'a', 'AAA' ]

const target = 'color colour';

// 'colo' 다음 'u'가 최대 한번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열을 검색함
const regexp6 = /colou?r/gi;

target.match(regexp6); // [ 'color', 'colour' ]
```

- `|` 은 or을 의미함
- `[]` 내의 문자는 or로 동작한다.
- `[]` 내에서 `-`를 사용하여 범위를 지정할 수 있다.

```javascript
const target = 'Aa Bb 56,000 +$%&';

const regexp = /A+|B+/gi;

target.match(regexp); // [ 'Aa', 'Bb' ]

// []내 문자열 or 의미
const regexp2 = /[AB]+/gi;

target.match(regexp2); // [ 'Aa', 'Bb' ]

const regexp3 = /[A-Z]/gi;

target.match(regexp3); // [ 'A', 'a', 'B', 'b' ]

// +를 함께 사용하면 분해되지 않은 단어로 검색할 수 있다.
const regexp4 = /[A-Z]+/gi;

target.match(regexp4); // [ 'Aa', 'Bb' ]
```

- `\d`는 숫자를 의미하고, `\D`는 숫자가 아닌 문자를 의미함
- `\w`는 알파벳, 숫자, 언더스코어를 의미하고, `\W`는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미함

  ```javascript
  const target = 'Aa Bb 56,000 +$%&';

  // 숫자 또는 ,가 한번 이상 반복되는 문자열 검색
  const regexp = /[\d,]+/g;

  target.match(regexp); // ['56,000']

  const regexp2 = /[\D,]+/g;

  target.match(regexp2); // [ 'Aa Bb ', ',', ' +$%&' ]

  const regexp3 = /[\w,]+/g;

  target.match(regexp3); // [ 'Aa', 'Bb', '56,000' ]

  const regexp4 = /[\W,]+/g;

  target.match(regexp4); // [ ' ', ' ', ',', ' +$%&' ]
  ```

- `[]` 내 `^`은 not을 의미함
- `[]` 밖 `^`은 문자열의 시작을 의미함

  ```javascript
  const target = '1234abcd';

  const regexp = /[^\d]/gi;

  target.match(regexp); // [ 'a', 'b', 'c', 'd' ]

  const regexp2 = /^[\d]/gi;
  target.match(regexp2); // ['1']
  ```

- `$` 문자열의 마지막 위치로 검색

  ```javascript
  const email = 'sejinee0918@gmail.com';

  const regexp = /com$/;

  regexp.test(email); // true
  ```
