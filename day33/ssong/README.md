# Day 33 - 레디스

# Redis

In-Memory 기반의 Dictionary(key-value) 구조 데이터 관리 시스템

일종의 NoSQL로 분류됨 (NoSQL이라는건 아님 주의)

일반 DB와 달리 디스크가 아닌 메모리에서 데이터를 처리하기 떄문에 속도가 빠름

## 자료구조

### String

Key → String 구조

```java
SET user:name:1 "길동"
GET user:name:1
DEL user:name:1
```

### List

key → [A, B, C, D] 구조

양쪽 끝에서 데이터를 삽입/삭제 할 수 있다 (큐, 스택 구현 가능)

```java
LPUSH queue A
LPUSH queue B
LPUSH queue C
RPUSH queue D

// [C, B, A. D]

LPOP queue
RPOP queue

// [B, A]
```

### Set

Key → {A, B, C} 구조로 중복되지 않는 값들의 집합

```java
SADD users 1
SADD users 2
SADD users 3

// 조회
SMEMBERS users

// 존재 확인
SISMEMBER users 1

// 삭제
SREM users 1

// 집합 연사 가능 SINTER, SUNION, SDIFF
```

### Hash

Key 하나에 여러 Field 값 저장 가능

```java
HSET user:1 name "길동"
HSET user:1 age 20
HSET user:1 email "test@test.com"

// 조회
HGET user:1 name
HGET user:1 age

HGETALL user:1

// 삭제
HDEL user:1 age
```

### Sorted Set (ZSet)

Set + Score

```java
// 추가
ZADD ranking 100 user1
ZADD ranking 80 user2
ZADD ranking 90 user 3

// 조회
ZRANGE ranking 0 -1

// 순위 조히
ZREVRANK ranking user1
```
