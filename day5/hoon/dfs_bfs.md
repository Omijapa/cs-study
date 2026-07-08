# day 5 DFS / BFS
## 1. 그래프 탐색이란
- 하나의 정점에서 시작, 연결된 정점들을 차례대로 방문하는 방법
- DFS, BFS가 대표적이다

### 기본 용어
- 정점(Vertex): 그래프를 구성하는 노드
- 간선(Edge): 정점과 정점을 연결하는 선
- 인접 정점: 현재 정점과 간선으로 직접 연결된 정점
- 방문 배열: 이미 방문한 정점인지 확인하기 위한 배열(visited)

## 2. BFS(Breadth-First Search, 너비 우선 탐색)
- 하나의 정점에서 시작하여 현재 정점과 가까운 정점부터 차례대로 방문하는 방식
-> 깊게 들어가기 전에 주변 정점들을 먼저 탐색

### 2.1. 특징
- 가까운 정점부터 탐색
- queue 이용
- 전체 탐색도 가능하지만, 시작 정점에서 다른 정점까지의 최단 거리를 구할 때 특히 자주 사용됨
- 간선의 가중치가 없는 그래프에서 최단 거리 문제에 적합

### 2.2. 동작 과정
1. 시작 정점 방문 처리
2. 시작 정점을 큐에 넣음
3. 큐에서 정점 하나 꺼냄
4. 꺼낸 정점과 연결된 정점 확인(방문한 정점인지 확인)
5. 아직 방문하지 않은 정점이면 방문 처리 후 큐에 넣음
6. 큐가 빌 때까지 반복

### 2.3. 코드(python)
```python
# 파이썬에는 큐가 구현되어 있으니 사용
from collections import deque
import sys

input = sys.stdin.readline 

n, m, start = map(int, input().split()) # 사용자가 1 2 3 입력 시 n = 1, m = 2, start = 3으로 입력됨

graph = [[] for _ in range(n + 1)] # 0 ~ n크기의 배열 생성(탐색할 그래프)

# 그래프 만들기(노드 1과 2가 연결되어 있을 경우 1 2 입력)
for _ in range(m):
    u,v = map(int, input().split())

    # 양방향 그래프(서로 인접해있음을 표시)
    graph[u].append(v)
    graph[v].append(u)

# 번호가 작은 정점부터 방문하기 위해 정렬
for i in range(1, n+1):
    graph[i].sort()

def bfs(start):
    queue = deque([start])
    visited[start] = True

    while queue: #큐가 빌 때까지 반복
        current = queue.popleft() # 큐에서 정점 하나 꺼냄
        print(current, end=" ") # 방문한 정점 출력
        
        for next_node in graph[current]:
            if not visited[next_node]: # 꺼낸 정점과 연결된 정점 확인(방문한 정점인지 확인)
                # 아직 방문하지 않은 정점이면 방문 처리 후 큐에 넣음
                visited[next_node] = True
                queue.append(next_node)

visited = [False] * (n + 1)
bfs(start)
'''
입력 예시
5 5 3
5 4
5 2
1 2
3 4
3 1

출력 결과
3 1 4 2 5
'''
```

## 3. DFS(Depth-First Search, 깊이 우선 탐색)
- 하나의 정점에서 시작하여 갈 수 있는 곳까지 깊게 방문.
- 더 이상 갈 수 없으면 다시 돌아와 다른 경로 탐색
=> 주변을 먼저 탐색하기보다 한 방향으로 깊게 탐색

### 3.1. 특징
- 한 방향으로 갈 수 있을 때까지 깊게 탐색
- 재귀 함수 또는 스택 이용
- 모든 경로를 탐색해야 할 때 자주 사용됨
- 백트래킹, 연결 요소 탐색 등에 활용

### 3.2. 동작 과정
1. 현재 정점 방문 처리
2. 현재 정점 출력
3. 현재 정점과 연결된 정점 확인
4. 아직 방문하지 않은 정점이면 재귀 호출
5. 더 이상 방문할 정점이 없으면 이전 정점으로 돌아감

### 3.3. 코드(python)
```python
import sys

input = sys.stdin.readline

n, m, start = map(int, input().split())

graph = [[] for _ in range(n+1)]

# 그래프 만들기
for _ in range(m):
    u, v = map(int, input().split())

    # 양방향 그래프
    graph[u].append(v)
    graph[v].append(u)

# 번호가 작은 정점부터 방문하기 위해 정렬
for i in range(1, n+1):
    graph[i].sort()

def dfs(current):
    visited[current] = True
    print(current, end = " ")

    for next_node in graph[current]:
        if not visited[next_node]:
            dfs(next_node) # 재귀
    

visited = [False] * (n + 1)
dfs(start)

'''
입력 예시
5 5 3
5 4
5 2
1 2
3 4
3 1

출력 결과
3 1 2 5 4
'''
```



## 4. DFS와 BFS 비교

| 구분 | DFS | BFS |
| --- | --- | --- |
| 탐색 방식 | 깊게 탐색 | 넓게 탐색 |
| 사용 구조 | 재귀 함수 또는 스택 | 큐 |
| 적합한 상황 | 모든 경로 탐색, 백트래킹 | 최단 거리, 최소 이동 횟수 |
| 방문 순서 | 한 방향으로 끝까지 | 가까운 정점부터 |

## 5. 시간 복잡도

| 그래프 표현 방식 | 시간 복잡도 |
| --- | --- |
| 인접 행렬 | O(V²) |
| 인접 리스트 | O(V + E) |

- DFS와 BFS 모두 정점과 간선을 한 번씩 확인한다.
- 현재 코드처럼 인접 리스트를 사용하면 시간 복잡도는 `O(V + E)`이다.
- 단, 정점 번호가 작은 순서대로 방문하기 위해 각 인접 리스트를 정렬하면 정렬 비용이 추가로 발생할 수 있다.

## 참고
### BFS가 큐를 사용하는 이유
- BFS는 먼저 발견한 정점 먼저 처리
=> FIFO 구조인 큐를 사용

### import sys   /   input = sys.stdin.readline
- 파이썬에서 입력을 더 빠르게 받기 위해 쓰는 코드
- `input = sys.stdin.readline`은 앞으로 `input()`을 호출할 때 `sys.stdin.readline()`을 사용하겠다는 의미

### n, m, start = map(int, input().split()) 동작 과정
※ 절대 내가 몰라서 넣은게 맞음
1. input(): 사용자로부터 문자열 입력받음
입력 예시: 1 2 3
2. .split(): 입력받은 문자열을 공백 기준으로 쪼갬
결과: ['1', '2', '3']
3. map(int, ...): 쪼개진 문자열 리스트의 요소를 하나씩 int형으로 변환
결과: 1, 2, 3(정수 상태)
4. n, m, start = : Unpacking 기능을 통해 변수에 순서대로 대입함
결과: n = 1, m = 2, start = 3이 각각 저장됨

### graph 배열 구조
- `graph`는 리스트 안에 리스트가 들어 있는 2차원 리스트 형태
- 각 인덱스는 정점 번호를 의미함
- 내부 리스트에는 해당 정점과 연결된 정점들이 저장됨
(ex) `graph[1] = [2, 3]`이면 1번 정점은 2번, 3번 정점과 연결되어 있다는 뜻임

### queue.popleft()
- 큐의 가장 앞에 있는 값을 꺼내는 함수
- BFS는 먼저 들어온 정점을 먼저 처리해야 하므로 `popleft()`를 사용한다.
- 리스트에서 `pop(0)`을 사용할 수도 있지만, 비효율적이므로 `deque.popleft()`를 사용하는 것이 좋다.
    - `list.pop(0)`: 맨 앞 제거 후 나머지 요소를 앞으로 이동, O(N)의 시간복잡도
    - `deque.popleft()`: 맨 앞 요소만 바로 제거, O(1)의 시간 복잡도


### DFS가 재귀를 사용하는 이유
- DFS는 현재 정점에서 다음 정점으로 이동하고, 그 정점에서 다시 다음 정점으로 이동하는 구조
- 이 과정이 함수가 자기 자신을 다시 호출하는 재귀 구조와 잘 맞음