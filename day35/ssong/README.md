# Day 35 - 해시 테이블

해시 테이블은 Key를 통해 값을 빠르게 저장 및 조회하는 자료구조이다

Java에서 HashMap, HashSet이 대표적인 구현체이다

해시 테이블을 단순하게 표현하면 아래 그림과 같다

```java
             Hash Table
        ┌───────┬─────────┐
index   │ key   │ value   │
────────┼───────┼─────────┤
  0     │       │         │
  1     │       │         │
  2     │       │         │
  3     │ "Kim" │ 100     │
  4     │       │         │
  5     │       │         │
  6     │ "Lee" │ 200     │
  7     │       │         │
        └───────┴─────────┘
```

key → hash function → hash value → bucket의 index → value 순으로 데이터를 확인한다

키를 해시 함수에 넣어 저장된 위치를 파악하므로 매우 빠른 조회가 가능하다

| 연산 | 평균 | 최악 |
| ---- | ---- | ---- |
| 삽입 | O(1) | O(n) |
| 조회 | O(1) | O(n) |
| 삭제 | O(1) | O(n) |

### Java의 HashMap

```java
Map<String, Integer> map = new HashMap<>();

// 추가
map.put("apple", 100);

// 조회
int price = map.get("apple");

// 삭제
map.remove("apple");

// 존재 여부
map.containsKey("apple");

// 전체 삭제
map.clear();

// 크기
int size = map.size();

// 비어있는지 체크
boolean empty = map.isEmpty();
```

### Load Factor

현재 해시 테이블이 얼마나 차 있는지를 나타내는 비율

Java HashMap의 기본 load factor는 0.75로, 저장된 엔트리의 수가 `capacity * load factor`를 초과하면 resize가 발생한다

### hashCode()와 equals()

HashMap은 `hashCode()`를 이용해 Bucket을 찾고, `equals()`를 통해 해당 Bucket의 Key가 실제로 같은 Key인지 확인한다. `hashCode()`와 `equals()`의 차이가 뭔지 잘 확인해야함

`equals()`는 두 객체가 논리적으로 같은 객체인지를 비교하는 메서드

예를 들면

```java
String a = new String("hello");
String b = new String("hello");

System.out.println(a == b);      // false
System.out.println(a.equals(b)); // true
```

`==` 는 서로 같은 객체를 참조하는지 보는거고

`equals()`는 객체의 내용이 논리적으로 같은지 보는거임

`hashCode()`는 객체를 대표하는 정수값(hash value)를 반환함

```java
String str = "hello";

System.out.println(str.hashCode());

/*
"hello" -> hashCode() -> 99162322 이런식임
*/
```

`equals()`가 `true`이면 두 객체의 `hashCode()`는 반드시 같아야 한다

즉, `A.equals(B) == true`이면 `A.hashCode == B.hashCode()` 이다. (역은 성립 X)
