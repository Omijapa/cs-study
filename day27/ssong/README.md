# Day 27 - Stack과 Queue, Hash

# Stack과 Queue

- 스택과 큐 모두 선형 자료구조이다
- 시간 복잡도와 공간 복잡도 모두 O(n)이다

### Stack

- Last-in First-out 구조이다
- 가장 위에 있는 데이터를 가리키는 변수를 ‘top’ 이라고 한다
- 삽입 연산은 push, 삭제 연산은 pop을 사용한다
- 웹 브라우저 방문 기록, 실행 취소, 후위 표기법 계산 등에서 사용된다

```java
import java.util.Stack;

Stack<Integer> stack = new Stack<>();

stack.push(10);
stack.push(20);
stack.push(30);

System.out.println(stack.pop());  // 30
System.out.println(stack.peek()); // 20
```

### Queue

- First-in First-out 구조이다
- 큐의 처음(삭제되는 부분)을 front, 큐의 끝(삽입되는 부분)을 rear라고 부른다
- 삽입 연산은 enQueue, 삭제 연산은 deQueue를 사용한다
- 주로 대기열 시스템을 만들 때 사용한다

```java
import java.util.Queue;
import java.util.LinkedList;

// 보통 Queue 인터페이스 + LinkedList로 사용함
Queue<Integer> queue = new LinkedList<>();

queue.offer(10);
queue.offer(20);
queue.offer(30);

System.out.println(queue.poll());  // 10
System.out.println(queue.poll());  // 20
System.out.println(queue.peek());  // 30
```

# Hash

- 키를 해시 함수에 넣어서 나온 값을 이용해 데이터를 빠르게 저장하고 찾는 자료구조이다
- 평균적으로 검색, 삽입, 삭제를 O(1)에 수행할 수 있다

### 해시 함수

해시 함수는 키를 해시 값으로 변환하는 함수를 말한다

```java
String key = "apple";

int hash = key.hashCode();

System.out.println(hash);
```

여기서 얻은 해시 값을 이용해 저장할 버킷을 정한다

### 해시 충돌

버킷의 수는 제한적이기 때문에 충돌은 발생할 수 밖에 없다. 이를 해결하기 위해 대표적으로 두 가지 방식이 존재한다

#### Chaining

하나의 버킷에 여러 데이터를 연결해서 저장하는 방식이다

Java의 HashMap도 이런 방식으로 충돌을 처리한다.

저장 구조를 LinkedList가 아닌 트리 구조를 사용하면 성능을 개선할 수 있다

#### Open Addressing

충돌이 발생하면 다른 빈 공간을 찾아 저장하는 방식이다.

대표적으로 Linear Probing, Quadratic Probing, Double Hashing 방법이 존재한다

### HashMap vs HashTable

|            | HashMap            | Hashtable        |
| ---------- | ------------------ | ---------------- |
| 동기화     | X                  | O                |
| null Key   | 1개 허용           | 허용하지 않음    |
| null Value | 허용               | 허용하지 않음    |
| 성능       | 일반적으로 더 좋음 | 동기화 비용 존재 |
| 현재 사용  | 많이 사용          | 레거시           |

### HashMap vs HashSet

HashMap은 Key+Value 저장, HashSet은 중복을 허용하지 않는 값의 집합
