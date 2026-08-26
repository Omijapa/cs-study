# LCA

Date: 2026년 8월 24일
Status: Done

# 개념

<aside>
📜

**LCA (Lowest Common Ancestor)**

두 노드의 공통된 조상 중에서 가장 가까운 조상을 찾는 문제

</aside>

---

# 구현

- BFS로 트리를 구성하고 깊이를 측정한다.
    - 모든 노드의 level을 구하고, 각 노드의 부모 노드를 parent[i][0]에 저장
- Sparse Table을 만들어서 2의 제곱수 단위로 부모 노드를 뛰어넘을 수 있게 한다.
- 두 노드 중 깊은 것을 끌어올려서 두 노드의 level을 맞추고, 두 노드를 동시에 끌어올려서 부모가 같은지 확인한다.

```python
import sys
from collections import deque

sys.setrecursionlimit(100000)
input = sys.stdin.readline

# N이 최대 50,000이므로 2^16(65536)이면 모든 깊이를 커버할 수 있음
LOG = 17 

def solve():
    N = int(input())
    
    # 트리 그래프 초기화
    graph = [[] for _ in range(N + 1)]
    for _ in range(N - 1):
        a, b = map(int, input().split())
        graph[a].append(b)
        graph[b].append(a)
        
    # parent[i][j]: i번 노드의 2^j 번째 위(부모) 노드
    parent = [[0] * LOG for _ in range(N + 1)]
    depth = [0] * (N + 1)
    visited = [False] * (N + 1)
    
    # BFS를 이용하여 각 노드의 깊이(depth)와 1번째 부모(2^0) 노드 구하기
    queue = deque([1])
    visited[1] = True
    
    while queue:
        curr = queue.popleft()
        for nxt in graph[curr]:
            if not visited[nxt]:
                visited[nxt] = True
                depth[nxt] = depth[curr] + 1
                parent[nxt][0] = curr
                queue.append(nxt)
                
    # Sparse Table 채우기
    # j를 1부터 LOG-1까지 늘려가며 2^j 번째 부모를 기록
    for j in range(1, LOG):
        for i in range(1, N + 1):
            # i의 2^j 번째 부모는, i의 2^(j-1) 번째 부모의 2^(j-1) 번째 부모와 같습니다.
            parent[i][j] = parent[parent[i][j-1]][j-1]
            
    # LCA 쿼리 처리
    M = int(input())
    for _ in range(M):
        a, b = map(int, input().split())
        
        # 항상 a가 더 깊은 노드가 되도록 스왑
        if depth[a] < depth[b]:
            a, b = b, a
            
        # [Step 1] a와 b의 깊이(depth) 동일하게 맞추기
        for i in range(LOG - 1, -1, -1):
            if depth[a] - depth[b] >= (1 << i):
                a = parent[a][i]
                
        # 깊이를 맞췄더니 두 노드가 일치하면 그것이 최소 공통 조상
        if a == b:
            print(a)
            continue
            
        # 공통 조상 찾기
        # 큰 보폭(2^16)부터 작은 보폭(2^0)까지 줄여가며 두 노드의 부모가 다를 때만 위로 이동
        for i in range(LOG - 1, -1, -1):
            if parent[a][i] != parent[b][i]:
                a = parent[a][i]
                b = parent[b][i]
                
        print(parent[a][0])

if __name__ == '__main__':
    solve()
```