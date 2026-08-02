# Day22-1 REST

## 1. REST란

#### 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것

1. HTTP URI(Uniform Resource Identifier)를 통해 자원을 명시하고
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것

### REST에서의 CRUD Operation

- Create: 데이터 생성(POST)
- Read: 데이터 조회(GET)
- Update: 데이터 수정(PUT, PATCH)
- Delete: 데이터 삭제(DELETE)


## 2. REST 구성요소

1. 자원(Resource): HTTP URI
2. 자원에 대한 행위(Verb): HTTP Method
3. 자원에 대한 행위의 내용(Representations): HTTP Message Pay Load


## 3. REST 특징

1. Server-Client 구조
2. Stateless(무상태)
3. Cacheable(캐시 처리 가능)
4. Layered System(계층화)
5. Uniform Interface(인터페이스 일관성)

| 특징                | 적용 내용                         |
| ----------------- | ----------------------------- |
| Server-Client     | 클라이언트가 요청하고 서버가 처리            |
| Stateless         | 요청마다 인증 정보 등을 함께 전달           |
| Cacheable         | 사용자 정보를 일정 시간 캐시 가능           |
| Layered System    | 중간에 인증 서버나 프록시를 둘 수 있음        |
| Uniform Interface | URI와 HTTP Method를 일관된 방식으로 사용 |


## 4. REST 장단점

### 장점
- HTTP 프로토콜 인프라 그대로 사용 -> REST API 사용을 위한 별도의 인프라 구출 필요 없음
- HTTP 프로토콜의 표준을 최대한 활용 -> 여러 추가적인 장점 함께 가져가도록 함
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능
- Hypermedia API의 기본을 충실히 지키면서 범용성 보장
- REST API 메시지가 의도하는 바를 명확하게 나타냄, 파악하기 쉬움
- 여러가지 서비스 디자인에서 생길 수 있는 문제 최소화
- 서버와 클라이언트의 역할 명확히 분리

### 단점
- 표준 자체가 존재하지 않아 정의가 필요
- HTTP Method 형태 제한적
- 브라우저를 통해 테스트할 경우가 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보 값을 처리해야 하므로 전문성이 요구됨
- 구형 브라우저 호환 x (그 구형을 쓰는 사람이 있다면 ☠️☠️☠️)


## 5. REST API
- REST의 원리를 따르는 API

### 설계 규칙
1. URI는 동사보다 명사, 대문자보다는 소문자

```text
Bad Example: http://khj93.com/Running/
Good Example:  http://khj93.com/run/  
```


2. 마지막에 슬래시(/) 포함 x
3. 언더바 대신 하이픈 사용
4. 파일확장자는 URI에 포함하지 않음

```text
Bad Example: http://khj93.com/photo.jpg  
Good Example:  http://khj93.com/photo 
```


5. 행위를 포함하지 않음
```text
Bad Example: http://khj93.com/delete-post/1  
Good Example:  http://khj93.com/post/1  
```


## 참고

### URL과 URI의 차이
- 엄밀히 말하면 URL은 URI의 한 종류이다.
- URI: 자원을 식별하는 포괄적인 개념
- URL: 자원의 위치를 나타내는 URI

### Server-Client 구조
- 서버와 클라이언트의 역할을 분리하는 구조이다.
- Client: 서버에 필요한 데이터를 요청하고 화면을 표시
- Server: 요청을 처리하고 데이터를 반환

### Stateless - 무상태
- 서버가 클라이언트의 이전 요청 상태를 저장하지 않는다는 특징
- 각각의 요청은 서로 독립적이며, 서버가 요청을 처리하는 데 필요한 정보가 요청 안에 모두 포함되어야 함.
> 첫 번째 요청과 두 번째 요청은 각각 독립적으로 처리됨


[출처](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80#google_vignette)

