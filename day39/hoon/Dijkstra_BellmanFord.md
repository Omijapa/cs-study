# Day39 다익스트라, 벨만포드

- 둘 다 **한 정점에서 다른 모든 정점까지의 최단 거리를 구하는 알고리즘**

## 다익스트라 알고리즘
- 시작 정점에서 다른 모든 정점까지의 최단 거리를 구함
- **음수 가중치가 있으면 사용 불가**
- 현재까지 거리가 가장 짧은 정점 선택하고, 그 정점을 거쳐 가는 경로를 갱신

### 코드
```text
1. 시작점 거리 = 0
2. 가장 가까운 정점을 하나 꺼냄
3. 그 정점을 거쳐 가면 더 짧아지는지 확인
4. 더 짧으면 거리 갱신
```


```python
import heapq

def dijkstra(graph, start):
    INF = float('inf') # 무한대로 설정
    distance = [INF] * len(graph) # 무한대로 초기화
    distance[start] = 0 # distance = [0, INF, INF]

    pq = []
    heapq.heappush(pq, (0, start))

    while pq:
        dist, now = heapq.heappop(pq)

        #  이미 더 짧은 경로가 발견된 경우
        if distance[now] < dist:
            continue
        
        for next_node, cost in graph[now]:
            new_cost = dist + cost

            if new_cost < distance[next_node]:
                distance[new_node] = new_cost
                heapq.heappush(pq, (new_cost, next_node))

        return distance

    graph = [
    [(1, 2), (2, 5)],  # 0 -> 1 비용 2, 0 -> 2 비용 5
    [(2, 1)],          # 1 -> 2 비용 1
    []
    ]

print(dijkstra(graph, 0))
# [0, 2, 3]
```


```text
graph[0] = [(1, 2), (2, 5)]

0 → 1 : 비용 2
0 → 2 : 비용 5
```



## 벨만포드 알고리즘
- 시작 정점에서 모든 정점까지의 최단 거리 구함
- **음수 가중치도 처리 가능**
- **음수 사이클 존재 여부도 확인 가능**
- 모든 간선을 반복해서 확인하면서 최단 거리를 갱신


### 코드
```text
1. 시작점 거리 = 0
2. 모든 간선을 확인하면서 거리 갱신
3. 이 과정을 V-1번 반복
4. 한 번 더 했는데도 갱신되면 음수 사이클
```


```python
def bellman_ford(n, edges, start):
    INF = float('inf')
    distance = [INF] * n # distance = [INF, INF, INF]
    distance[start] = 0 # distance = [0, INF, INF]

    # 정점 개수 - 1 만큼 반복
    for _ in range(n - 1):
        for u, v, cost in edges:
            if distance[u] == INF:
                continue

            if distance[v] > distance[u] + cost:
                distance[v] = distance[u] + cost

    # 음수 사이클 확인
    for u, v, cost in edges:
        if distance[u] == INF:
            continue

        if distance[v] > distance[u] + cost:
            return None  # 음수 사이클 존재

    return distance


edges = [
    (0, 1, 4),
    (0, 2, 5),
    (1, 2, -3)
] # (출발 정점, 도착 정점, 비용)

print(bellman_ford(3, edges, 0))
# [0, 4, 1]
```

## 비교
|           | 다익스트라          | 벨만-포드          |
| --------- | -------------- | -------------- |
| 목적        | 단일 출발점 최단경로    | 단일 출발점 최단경로    |
| 음수 간선     | 불가능          | 가능           |
| 음수 사이클 탐지 | 불가능        | 가능              |
| 방식        | 가장 가까운 정점부터 확정 | 모든 간선을 반복해서 갱신 |
| 시간복잡도     | `O((V+E)logV)` | `O(VE)`        |
| 속도        | 빠름             | 상대적으로 느림       |

- 다익스트라 = 가장 가까운 정점부터
- 벨만포드 = 모든 간선을 V-1번

## 참고

### heapq.heappush()
- 우선순위 큐에 값을 넣는 함수

```python
import heapq

pq = []

heapq.heappush(pq, 5)
heapq.heappush(pq, 2)
heapq.heappush(pq, 7)

print(heapq.heappop(pq)) # 출력 2(가장 작은 값 꺼내줌)
```
> 파이썬의 heapq는 기본적으로 가장 작은 값을 먼저 꺼내는 우선순위 큐로 사용