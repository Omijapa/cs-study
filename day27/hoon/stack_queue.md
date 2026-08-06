# day 27-1 Stack, Queue

## Stack
- LIFO(Last In, First Out)
- 파이썬에서는 일반적으로 `list` 사용

```python
stack = []

# 데이터 삽입: push
stack.append(10)
stack.append(20)
stack.append(30)

print(stack)  # [10, 20, 30]

# 데이터 삭제 및 반환: pop
value = stack.pop()

print(value)  # 30
print(stack)  # [10, 20]
```


- 스택의 가장 위 데이터를 삭제하지 않고 확인하려면 `stack[-1]` 사용 
```python
print(stack[-1])  # 20
```


## Queue
- FIFO(First In, First Out)
- 파이썬에서는 `collections.deque` 사용이 효율적

```python
from collections import deque

queue = deque()

# 데이터 삽입: enqueue
queue.append(10)
queue.append(20)
queue.append(30)

print(queue)  # deque([10, 20, 30])

# 데이터 삭제 및 반환: dequeue
value = queue.popleft()

print(value)  # 10
print(queue)  # deque([20, 30])
```


| 자료구조 | 데이터 처리 방식         | 삽입         | 삭제          |
| ---- | ----------------- | ---------- | ----------- |
| 스택   | LIFO: 나중에 들어온 것부터 | `append()` | `pop()`     |
| 큐    | FIFO: 먼저 들어온 것부터  | `append()` | `popleft()` |
