# Day 24 - 로드밸런싱

# 로드밸런싱

로드 밸런싱은 여러 서버에 들어오는 요청들을 적절하게 분산하여 특정 서버에 부하가 집중되지 않도록 하는 기술이다.

구조도를 그리면 다음과 같다.

```
                 ┌─ Server A
Client ─→ Load Balancer ─ Server B
                 └─ Server C
```

## 필요한 이유

1. 고가용성 : 일부 서버에 장애가 발생해도 서비스 유지 (사용 가능한 서버로 트래픽을 리다이렉션)
2. 확장성 : 서버를 추가하여 증가하는 트래픽에 대응
3. 보안성 : 트래픽 모니터링 및 악성 콘텐츠 차단, 공격 트래픽 차단, 추가 보안 레이어 추가
4. 성능 향상 : 여러 서버가 요청을 나눠서 처리 혹은 더 가까이있는 서버로 리다이렉션

## 로드 밸런싱 알고리즘

### 정적 로드 밸런싱

#### Round Robin

서버를 순서대로 돌아가면서 요청을 전달

구현이 단순하고, 서버들의 처리 능력이 비슷한 경우 효과적임

#### Weighted Round Robin

서버마다 가중치를 부여해 성능이 더 좋은 서버에 더 많은 요청을 전달

#### IP Hash

클라이언트의 IP 주소를 해싱하여 특정 서버를 결정

같은 IP의 클라이언트를 동일한 서버로 연결되도록 만들 수 있다는 장점이 있음

### 동적 로드 밸런싱

#### Least Connections

현재 연결된 요청 수가 가장 적은 서버에 새로운 요청을 전달

요청마다 처리 시간이 크게 다른 경우 유리함

#### Weighted Least Connections

일부 서버가 다른 서버보다 더 많은 연결을 처리할 수 있다고 가정해 서버 별로 다른 가중치를 할당하여 용량별 연결이 가장 적은 서버로 요청을 전달함

#### Least Response

서버 응답 + 활성 연결을 결합해 최상의 서버를 결정

#### Resource-based

서버 리소스의 사용량을 계산해 해당 서버에 충분한 여유가 있는지 확인후 요청을 전달

## L4 vs L7 Load Balancing

### L4 Load Balancer

OSI 계층 중 전송 계층의 정보를 이용한다. (IP, Port 등)

패킷을 깊게 분석하지 않기 때문에 일반적으로 빠르고 단순하다.

### L7 Load Balancer

OSI 계층 중 응용 계층의 정보를 기반으로 요청을 분산한다. (URL, HTTP Header, Cookie, Host, HTTP Method 등)

### L4와 L7 로드 밸런서의 차이

예를 들어 다음 요청이 들어왔다고 해보자.

```
https://example.com/api/orders/123
```

L4는 "10.0.0.1의 443번 포트로 TCP 연결이 들어왔네.” 라고 이해를 하고, L7은 더 구체적으로 "example.com의 /api/orders/123으로 HTTP 요청이 들어왔네.” 라고 이해를 한다.

즉, L4는 IP, Port, TCP/UDP와 같은 정보만 보고 분산하고 L7은 URL, HTTP Header, Cookie 같은 HTTP 정보까지 보고 분산한다.

쇼핑몰 서버가 3대가 있다고 하자.

```
              ┌─ Web Server A
사용자 → L4 LB	├─ Web Server B
			  └─ Web Server C
```

그리고 세 서버는 모두 동일한 기능을 제공한다고 하자.

```
Web Server A
├─ 주문 기능
├─ 사용자 기능
└─ 이미지 제공
Web Server B
├─ 주문 기능
├─ 사용자 기능
└─ 이미지 제공

Web Server C
├─ 주문 기능
├─ 사용자 기능
└─ 이미지 제공
```

사용자 요청들이 다음과 같이 있을 때,

```
사용자 1
https://example.com/api/orders/123

사용자 2
https://example.com/api/users/10

사용자 3
https://example.com/images/shoes.png
```

L4의 관점에서는 모두 목적지가 10.0.0.1이고 443 포트로 들어오는 TCP 요청이다. 그래서 L4는 연결을 적절한 서버로 분산시킨다.

하지만 만약 서비스가 커져서 Order Server, User Server, Image Server 이렇게 서버 별로 역할이 나뉜 경우, 같은 요청이 들어왔을 때 L4는 요청을 적절하게 처리할 수가 없다. order는 주문 서버로 보내야 하는데 L4 관점에서는 세 요청이 모두 동일하기 때문이다.

반면 L7은 HTTP 요청 내용을 볼 수 있다

```
사용자
  │
  ▼
┌─────────────────────┐
│       L7 LB         │
│                     │
│ URL을 확인한다         │
└──────────┬──────────┘
           │
     ┌─────┼─────┐
     ▼     ▼     ▼

 /orders  /users  /images
    │       │       │
    ▼       ▼       ▼
 Order    User    Image
 Server   Server   Server
```

즉 위와 같이 처리할 수 있는 것이다. (이걸 Path-based Routing 이라고 부른다)

이후 /orders 요청을 처리하는 여러 서버들 중 하나로 트래픽을 분산시킨다.

L4가 위와 같이 구현하려면 반드시 도메인 별 서버가 포트 번호로 구분이 되어야만 한다.
