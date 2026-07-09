# day 7 LRU Cache(Least Recently Used)

## 1. Cache란?
- 자주 사용하는 데이터나 값을 미리 저장해두는 임시 저장 공간
- 원래 데이터를 가져오는 비용이 크거나, 값을 다시 계산하는 시간이 오래 걸릴 때 캐시를 사용하면 더 빠르게 데이터에 접근할 수 있음
- 캐시는 저장 공간이 유한 -> 공간이 가득 찼을 때 어떤 데이터를 제거할지 결정하는 기준 필요 -> `LRU`

## 2. LRU Cache란?
- LRU: Least Recently Used; 가장 오랫동안 사용하지 않은 데이터를 우선적으로 제거하는 방식
    - 캐시에 공간이 부족할 경우 가장 오랫동안 사용하지 않은 데이터를 삭제하고, 새로운 데이터를 삽입함

### LRU Cache 기본 규칙
1. 데이터 조회 / 삽입 -> 해당 데이터는 `최근에 사용한 데이터`가 됨
2. 최근에 사용한 데이터는 앞쪽으로 이동
3. 캐시 용량이 초과되면 가장 오래 사용하지 않은 데이터를 제거

## 3. LRU Cache 구현 방식
### 3.1. HashMap
- key를 통해 노드에 빠르게 접근

### 3.2. Doubly Linked List
- 데이터의 사용 순서를 관리
- head에 가까운 노드: 최근에 사용한 데이터
- tail에 가까운 노드: 오래 사용하지 않은 데이터(제거 우선 대상)
```text
    head <-> 최근 사용 데이터 <-> ... <-> 오래된 데이터 <-> tail
```
- 어떤 데이터가 사용됨 -> 해당 노드를 head쪽으로 옮김
- 캐시 용량이 초과됨 -> tail 바로 앞에 있는 노드 제거


## 4. 주요 연산 (알고리즘 흐름)
### 4.1. get(key)
1. HashMap에 `key`가 있는지 확인
2. 없으면 `-1` 반환
3. 있으면 해당 노드를 최근 사용한 노드로 갱신
4. 노드의 `value` 반환

### 4.2. put(key, value)
1. 이미 존재하는 key라면 value를 갱신한다
2. 해당 노드를 최근 사용한 노드로 이동시킨다(head 쪽)
3. 새로운 key라면 새 노드를 추가한다.
4. 캐시 용량을 초과하면 가장 오래 사용하지 않은 노드(tail쪽)를 제거한다.

## 5. 코드(python)
```python
class Node:
    def __init__(self, key = 0, value = 0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None
    

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity # 캐시 용량
        self.cache = {} # hashMap

        # 더미 head, tail 노드
        self.head = Node()
        self.tail = Node()

        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node: Node) -> None:
        # 연결 리스트에서 node를 제거
        prev_node = node.prev
        next_node = node.next

        prev_node.next = next_node
        next_node.prev = prev_node

    def _add_to_head(self, node: Node) -> None:
        # node를 head 바로 뒤에 추가
        first_node = self.head.next

        node.prev = self.head
        node.next = first_node


        self.head.next = node
        first_node.prev = node
    
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1 # key가 캐시에 없는 경우 -1 반환
        
        node = self.cache[key] # HashMap을 통해 key에 해당하는 Node를 바로 찾음

        # 사용했으므로 최근 사용 위치로 이동
        self._remove(node)
        self._add_to_head(node)

        return node.value
    
    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.value = value # value 업데이트

            # 값이 갱신되었으므로 최근 사용 위치로 이동
            self._remove(node)
            self._add_to_head(node)

        else:
            # key가 캐시에 없을 경우 넣기
            new_node = Node(key, value)
            self.cache[key] = new_node
            self._add_to_head(new_node)

            # 용량 초과 시 가장 오래 사용하지 않은 노드 제거
            if len(self.cache) > self.capacity:
                lru_node = self.tail.prev # 끝쪽 노드

                self._remove(lru_node) # linked list에서 삭제
                del self.cache[lru_node.key] # linked list에서 지운 노드를 HashMap에서도 삭제
```
### 구현 예시
1. cache = LRUCache(2)
```text
Linked List: head <-> tail
HashMap: {}
```

2. put(1,10)
```text
Linked List: head <-> Node(1, 10) <-> tail
HashMap: { 1: Node(1, 10) }
```

3. put(2, 20)
```text
Linked List: head <-> Node(2, 20) <-> Node(1, 10) <-> tail
HashMap: { 1: Node(1, 10), 2: Node(2, 20) }
```

4. get(1)
```text
Node(1, 10)을 사용했으므로 최근 사용 위치인 head 뒤로 이동시킨다.

Linked List: head <-> Node(1, 10) <-> Node(2, 20) <-> tail
HashMap: { 1: Node(1, 10), 2: Node(2, 20) }
```

5. put(3, 30)
```text
캐시 용량은 2인데 현재 데이터가 3개이므로 용량을 초과
head <-> Node(3, 30) <-> Node(1, 10) <-> Node(2, 20) <-> tail
                                            ↑
                                         제거 대상

연결리스트, HashMap에서 삭제
- self._remove(lru_node)
- del self.cache[lru_node.key]

Linked List: head <-> Node(3, 30) <-> Node(1, 10) <-> tail
HashMap: { 1: Node(1, 10), 3: Node(3, 30) }
```

6. get(2)
```text
앞에서 Node(2, 20)은 제거되었으므로 HashMap에 존재하지 않는다
-> -1 반환
```

## 6. 시간복잡도 / 공간복잡도
### 6.1. 시간복잡도
- get: O(1)
- put: O(1)
HashMap의 조회가 평균 O(1)이기 때문에 get, put 모두 평균 O(1)에 수행된다.

### 6.2. 공간복잡도
- 캐시에 최대 capacity개의 데이터가 저장됨
- 공간복잡도: O(capacity)


## 참고
### HashMap과 Doubly Linked List를 같이 사용하는 이유
- HashMap:
    - key를 이용해 데이터를 O(1)에 찾을 수 있음
    - 어떤 데이터가 가장 오래 사용되지 않았는지 순서 관리 어려움

- Linked List:
    - 데이터의 순서 관리하기 좋음
    - 특정 key를 가진 노드를 찾으려면 처음부터 탐색해야 함(O(n) 소요)

- HashMap으로 노드를 O(1)에 찾고, Doubly Linked List로 사용 순서를 O(1)에 변경할 수 있다.
-> LRU Cache의 get, put 연산을 모두 O(1)에 처리할 수 있음

### def get(self, key: int) -> int:
- key는 int 타입으로 받을 예정이고, get 함수는 int 타입 값을 반환할 예정이다.
- `->None`, `->int`: 파이썬 타입 힌트(type hint): 함수의 반환값 타입이 무엇인지 표시하는 용도

### HashMap: { 1: Node(1, 10) }
- HashMap에는 value 자체가 아니라, 연결 리스트에 있는 Node 객체의 참조가 저장된다.
```text
HashMap:
1 -> Node(1, 10)
```s