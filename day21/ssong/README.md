# Day 21 - HTTP vs HTTPS, HTTP 동작 과정과 메소드

# HTTP vs HTTPS

## HTTP

HTTP(Hypertext Transfer Protocol)는 클라이언트와 서버 간 통신을 위한 프로토콜이다.

기본 포트로 80을 사용한다.

### 특징

- Stateless : 서버는 이전 요청의 상태를 저장하지 않는다.
- Connectionless : 요청-응답이 끝나면 연결을 종료하는 특성을 가지며, HTTP/1.1부터는 Keep-Alive를 통해 연결을 재사용한다.

### 메서드

| 메서드  | 설명                                                                           |
| ------- | ------------------------------------------------------------------------------ |
| GET     | 리소스를 조회하기 위한 메서드                                                  |
| HEAD    | GET과 동일하지만 응답 본문(Body) 없이 헤더만 반환하는 메서드                   |
| POST    | 새로운 리소스를 생성하거나 서버에 데이터를 처리하도록 요청하는 메서드          |
| PUT     | 리소스 전체를 생성하거나 기존 리소스를 전체 대체(교체)하는 메서드              |
| PATCH   | 리소스의 일부만 수정하는 메서드                                                |
| DELETE  | 리소스를 삭제하는 메서드                                                       |
| OPTIONS | 서버가 지원하는 HTTP 메서드를 확인하거나 CORS Preflight 요청에 사용하는 메서드 |
| CONNECT | 프록시 서버와 터널을 생성하기 위한 메서드                                      |
| TRACE   | 요청이 서버까지 전달되는 과정을 확인하기 위한 진단용 메서드(대부분 비활성화)   |

### HTTP 동작 과정

1. 사용자가 URL 입력
2. DNS 조회
3. 서버와 연결(TCP, QUIC)
4. HTTP Request 전송
5. 서버 처리
6. HTTP Response 반환
7. 브라우저 렌더링

## HTTPS

HTTPS(Hypertext Transfer Protocol Secure)는 HTTP의 보안 계층을 추가한 프로토콜이다. 기본 포트로 443을 사용한다. 기존 HTTP는 암호화되지 않은 데이터를 전송해 중간에 제3자가 가로채고 읽을 수 있었는데 HTTPS는 HTTP 위에 TLS를 적용하여 데이터를 암호화해 전송한다.

### 특징

1. 기밀성 (Confidentiality)
2. 무결성 (Integrity)
3. 인증 (Authentication)

### HTTPS 동작과정 (TLS Handshake)

1. 클라이언트가 서버에 `Client Hello` 전송
2. 서버는 `Server Hello`와 인증서 전송
3. 브라우저가 인증서 검증
4. TLS Handshake를 통해 세션 키를 안전하게 교환
5. 이후 세션 키로 암호화 통신
