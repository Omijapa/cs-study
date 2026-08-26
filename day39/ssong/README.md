# Day 39 - 다익스트라, 벨만포드

대표적인 최단 거리 알고리즘

| 알고리즘                 | 대상                    | 음수 간선 | 시간 복잡도        |
| ------------------------ | ----------------------- | --------- | ------------------ |
| BFS                      | 가중치가 동일한 그래프  | ❌        | `O(V + E)`         |
| 다익스트라 (우선순위 큐) | 단일 시작점 → 모든 정점 | ❌        | `O((V + E) log V)` |
| 벨만-포드                | 단일 시작점 → 모든 정점 | ✅        | `O(VE)`            |
| 플로이드-워셜            | 모든 정점 → 모든 정점   | ✅        | `O(V³)`            |

# 다익스트라 (Dijkstra)

가중치가 있는 그래프에서 하나의 시작 정점으로부터 다른 모든 정점까지의 최단 거리를 구하는 알고리즘

1. 시작 정점 설정
2. 시작 정점을 기준으로 각 정점으로 가는 최소 비용을 저장
3. 방문하지 않은 정점 중 가장 비용이 적은 정점을 선택
4. 해당 정점을 거쳐서 특정 정점으로 가는 경우를 고려해 최소 비용 갱신
5. 3~4 반복

### priority queue로 구현

```cpp
#include <iostream>
#include <vector>
#include <functional>

using namespace std;

const int INF = 1e9; // 충분히 큰 값

int main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr);

	int V, E;
	cin >> V  >> E;

	int start;
	cin >> start;

	vector<vector<pair<int, int>>> graph(V+1);

	for (int i = 0; i < E; i++) {
		int u, v, w;
		cin >> u >> v >> w;

		graph[u].push_back({v, w});
	}

	vector<int> dist(V+1, INF);

	// priority_queue< 데이터타입, 컨테이너, 정렬기준 > pq;
	priority_queue<
		pair<int, int>,  // {시작점으로부터의 거리, 노드 번호}
		vector<pair<int, int>>,
		greater<pair<int, int>>  // 오름차순으로 변경 (최소힙)
	> pq;

	dist[start] = 0;
	pq.push({0, start});

	while (!pq.empty()) {
		auth [curDist, cur] = pq.top();
		pq.pop();

		if (curDist != dist[cur]) continue;

		for (auto [next, const] : graph[cur]) {
			int nextDist = curDist + cost;

			if (nextDist < dist[next]) {
				dist[next] = nextDist;
				pq.push({nextDist, next});
			}
		}
	}

	for (int i = 1; i <= V; i++) {
		if (dist[i] == INF) cout << "INF\n";
		else cout << dist[i] << '\n';
	}
}
```

### priority queue 없이 구현

```cpp
vector<int> dijkstra(int start, int n, vector<vector<pair<int, int>>>& graph) {
    const int INF = 1e9;

    vector<int> dist(n + 1, INF);
    vector<bool> visited(n + 1, false);

    dist[start] = 0;

    for (int i = 1; i <= n; i++) {
        int cur = -1;

        // 방문하지 않은 정점 중 거리가 가장 짧은 정점 선택
        for (int j = 1; j <= n; j++) {
            if (!visited[j] && (cur == -1 || dist[j] < dist[cur])) {
                cur = j;
            }
        }

        if (cur == -1 || dist[cur] == INF)
            break;

        visited[cur] = true;

        // 인접 정점의 거리 갱신
        for (auto [next, cost] : graph[cur]) {
            dist[next] = min(dist[next], dist[cur] + cost);
        }
    }

    return dist;
}
```

# 벨만-포드(Bellman-Ford)

하나의 시작 정점에서 다른 모든 정점까지의 최단 거리를 구하는 알고리즘

다익스트라와 달리 음수 간선을 처리할 수 있고, 음수 사이클의 존재 여부를 확인할 수 있다

벨만-포드의 핵심은 ‘모든 간선을 반복해서 확인하면서 최단 거리를 갱신한다’는 것이다

1. 시작 노드 설정
2. 최단 거리 테이블 초기화
3. 다음 과정을 (V - 1)번 반복
   1. 모든 간선 E개를 하나씩 확인
   2. 각 간선을 거쳐 다른 노드로 가는 비용르 계산하여 최단 거리 테이블 갱신

- 음수 간선 순환이 발생하는지 확인하려면 3번 과정을 한번 더 수행한다. 이때 최단 거리 테이블이 갱신된다면 음수 간선 순환이 존재하는 것이다

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

const int INF = 1e9;

struct Edge {
	int from;
	int to;
	int cost;
};

int main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr);

	int V, E;
	cin >> V >> E;

	vector<Edge> edges;

	for (int i = 0 ; i < E; i++) {
		int from, to, cost;
		cin >> from >> to >> cost;

		edges.push_back({from, to, cost});
	}

	vector<int> dist(V + 1, INF);
	dist[1] = 0;

	for (int i = 1; i <= V - 1; i++) {
		for (auto [from, to, cost]: edges) {
			if (dist[from] == INF) continue;

			if (dist[to] > dist[from] + cost) {
				dist[to] = dist[from] + cost;
			}
		}
	}

	for (int i = 1; i <= V; i++) {
		if (dist[i] == INF) cout << "INF\n";
		else cout << dist[i] << '\n';
	}
}

```
