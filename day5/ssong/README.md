# Day 5 - DFS/BFS

# DFS (깊이 우선 탐색)

DFS는 루트 노드 혹은 임의의 노드에서 한 경로를 끝까지 탐색한 뒤, 더 이상 갈 곳이 없으면 가장 가까운 갈림길로 되돌아와 다른 경로를 탐색하는 알고리즘이다.

트리 순회 (pre/in/post order traversals) 방법들은 모두 DFS의 한 종류이다

![](https://www.fun-coding.org/00_Images/BFSDFS.png)

#### C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> graph[100];
bool visited[100];

void dfs(int node) {
	visited[node] = true;
	cout << node << " ";

	for (int next : graph[node]) {
		if (!visited[next]) dfs(next);
	}
}

int main() {
	graph[1].push_back(2);
  graph[1].push_back(3);
  graph[2].push_back(4);
  graph[2].push_back(5);

	dfs(1);
}
```

#### Java

```java
import java.util.*;

public class Main {
    static ArrayList<Integer>[] graph;
    static boolean[] visited;

    static void dfs(int node) {
        visited[node] = true;
        System.out.print(node + " ");

        for (int next : graph[node]) {
            if (!visited[next]) {
                dfs(next);
            }
        }
    }

    public static void main(String[] args) {
        graph = new ArrayList[100];
        visited = new boolean[100];

        for (int i = 0; i < 100; i++) {
            graph[i] = new ArrayList<>();
        }

        graph[1].add(2);
        graph[1].add(3);
        graph[2].add(4);
        graph[2].add(5);

        dfs(1);
    }
}
```

### 시간 복잡도

$O(V+E)$

정점(V) 방문 : $O(V)$

간선(E) 확인: $O(E)$

### 공간 복잡도

$O(V)$

방문 배열 : $O(V)$

재귀 호출 스택 : $O(V)$

### 장점

- 구현이 간단하다.
- 백트래킹 문제에 적합하다.
- 경로 탐색에 유리하다.
- 메모리를 상대적으로 적게 사용한다.

### 단점

- 최단 경로가 보장되지 않는다.
- 깊이가 매우 크면 Stack Overflow가 발생할 수 있다.
- 해답이 가까이 있어도 먼 곳까지 탐색하는 경우가 존재한다.

---

# BFS (너비 우선 탐색)

BFS는 현재 노드와 가까운 노드부터 차례대로 탐색하는 알고리즘이다. 현재 정점에서 방문할 수 있는 모든 미방문 정점들을 큐에 push 하고 다음 정점(큐에서 pop해서 얻음)에서도 동일한 과정을 반복한다.

#### C++

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

vector<int> graph[100];
bool visited[100];

void bfs(int start) {
    queue<int> q;

    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int cur = q.front();
        q.pop();

        cout << cur << " ";

        for (int next : graph[cur]) {
            if (!visited[next]) {
                visited[next] = true;
                q.push(next);
            }
        }
    }
}

int main() {
    graph[1].push_back(2);
    graph[1].push_back(3);
    graph[2].push_back(4);
    graph[2].push_back(5);

    bfs(1);
}
```

#### Java

```java
import java.util.*;

public class Main {
    static ArrayList<Integer>[] graph;
    static boolean[] visited;

    static void bfs(int start) {
        Queue<Integer> q = new LinkedList<>();

        q.offer(start);
        visited[start] = true;

        while (!q.isEmpty()) {
            int cur = q.poll();

            System.out.print(cur + " ");

            for (int next : graph[cur]) {
                if (!visited[next]) {
                    visited[next] = true;
                    q.offer(next);
                }
            }
        }
    }

    public static void main(String[] args) {
        graph = new ArrayList[100];
        visited = new boolean[100];

        for (int i = 0; i < 100; i++) {
            graph[i] = new ArrayList<>();
        }

        graph[1].add(2);
        graph[1].add(3);
        graph[2].add(4);
        graph[2].add(5);

        bfs(1);
    }
}
```

### 시간 복잡도

$O(V+E)$

정점(V) 방문 : $O(V)$

간선(E) 확인: $O(E)$

### 공간 복잡도

$O(V)$

방문 배열 : $O(V)$

큐 저장 공간 : $O(V)$

### 장점

- 최단 거리가 보장된다
- 레벨 별 탐색이 보장된다
- 미로 탐색, 최단 경로 문제에 적합하다

### 단점

- 큐 사용으로 인해 DFS보다 메모리 사용량이 많다
- 깊은 탐색에서는 비효율적이다
- 노드 수가 많으면 큐 크기가 매우 커질 수 있다

---
