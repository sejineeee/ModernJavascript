# REST API

로이필딩은 HTTP의 장점을 최대한 활용할 수 있는 아키텍처로서 REST를 소개했고, HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하고 있다.

REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처

REST API는 REST 아키텍처 스타일을 따르는 API를 의미한다.

## REST

### 구성
- 자원 - URI
- 행위 - HTTP 요청 메서드
- 표현

### 특징

- clinet-server

- stateless : 서버가 이전의 모든 요청과 독립적으로 모든 클라이언트 요청을 완료하는 통신 방법을 나타냄

- cache : 서버 응답 시간을 개선하기 위해 클라이언트 또는 중개자에 일부 응답을 저장하는 프로세스인 캐싱을 지원

- uniform interface

- layered system

- code-on-demand(optional)


#### Uniform Interface

1. identification of resources : URI를 통해 자원을 식별해야 된다.

```
GET /todos/1


* 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.

GET /getTodos/1 또는 GET /todos/show/1
이런 이름은 동사를 사용했기 때문에 좋지 않다고 볼 수 있다.
```


2. manipulation of resources through representations : 표현을 통한 자원의 조작

    HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다. 주로 5가지 요청 메서드 `GET, POST, PUT, PATCH, DELETE` 를 사용한다.


3. self-descriptive messages : 메시지는 스스로를 설명해야 한다.

    클라이언트와 서버 사이의 컴포넌트들은 메시지의 내용을 참고하여 적절한 작업을 수행한다.

    Host 헤더에 도메인명 기재 필요!

    ```
    GET /user/1 HTTP/1.1
    host: example.com
    ```
    HTTP/1.1부터 Host 헤더 필수화


4. hypermedia as the engine of application state(HATEOAS)

    애플리케이션의 상태는 하이퍼링크를 이용해 전이되어야 한다.

### RESTful API

REST 아키텍처를 구현하는 웹 서비스를 RESTful 웹 서비스라고 하고, RESTful API는 RESTful 웹 API를 의미한다.

---
- [REST API 제대로 알고 사용하기](https://meetup.nhncloud.com/posts/92)

- [RESTful API](https://aws.amazon.com/ko/what-is/restful-api/)

- [그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)