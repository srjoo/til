# 다익스트라(Dijkstra) 알고리즘
- 다이나믹 프로그래밍을 활용한 최단 경로 탐색 알고리즘
- 시작점에서 다른 모든 정점으로 가는 최단 경로를 알 수 있다

# 특징
- 다이나믹 프로그래밍(DP)을 사용한다
- 각 단계마다 탐색 노드로 한 번 선택된 노드는 최단 거리를 갱신하고, 그 뒤에는 더 작은 값으로 다시 갱신되지 않는다
- 다익스트라 알고리즘은 가중치가 양수일 때만 사용 가능하다

# 과정
0. '최단 거리 테이블'을 초기화한다.(초기화의 경우 가장 큰 값 Inf를 준다)
1. 시작점을 기준으로 각 노드의 최소 비용을 저장
2. 방문하지 않은 노드 중 가장 비용이 적은 노드를 선택(노드 방문 여부 체크 배열을 이용한다)
3. 해당 노드를 거쳐서 가는 다른 노드의 최소 비용을 갱신
4. 2~3번을 반복(도착점의 경우 계산을 할 필요가 없다)

# 구현
- 순차 탐색으로 구현
```c++
#include <vector>
#include <queue>

// 아직 방문하지 않은 노드 중 
// 가장 거리값이 작은 노드의 인덱스 반환
int FindSmallestNode()
{
	int min_dist = INF;
    int min_idx = -1;
    for (int i = 0; i<= N; i++)
    {
    	if (visited[i] == true)
        	continue;
        if (dist[i] < min_dist)
        {
        	min_dist = dist[i];
            min_idx = i;
        }
    }
    return min_idx;    
}

// 다익스트라
void Dijkstra()
{
	for (int i = 1; i <= N; i++)
    	dist[i] = map[start][i];  // 시작 노드와 인접한 정점에 대해 거리 계산
    
    dist[start] = 0;
    visited[start] = true;
    
    for (int i = 0; i < N - 1; i++)
    {
    	int new_node = FindSmallestNode();
        visited[new_node] = true;
        
        for(int j = 0; j <= N; j++)
        {
        	if (visited[j] == true)
            	continue;
            if (dist[j] > dist[new_node] + map[new_node][j])
            	dist[j] = dist[new_node] + map[new_node][j];
        }
    }
}
```

- 우선순위 큐로 구현(최소힙으로 구현하며, 최대힙의 경우 저장되는 값에 -를 붙여 음수로 만든다
```c++
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

# define INF 99999999

// 시작 위치와 정점의 개수, 인접 행렬을 받아
// 최소 거리 행렬을 반환함
vector<int> dijkstra(int start, int N, vector<pair<int, int>> graph[])
{
    vector<int> dist(N, INF);  // 거리를 저장할 리스트 초기화
    priority_queue<pair<int, int>> pq;  // 우선순위 큐 선언

    dist[start] = 0;  // 시작 노드 거리를 0으로 함
    pq.push({ 0, start });  // 우선순위 큐에 넣기

    while (!pq.empty())
    {
        int cur_dist = -pq.top().first; // 큐에서 꺼내 방문하고 있는 정점의 거리
        int cur_node = pq.top().second;  // 정점의 인덱스(번호)
        pq.pop();

        for (int i = 0; i < graph[cur_node].size(); i++)
        {
            int nxt_node = graph[cur_node][i].first;  // 인접 정점 번호
            int nxt_dist = cur_dist + graph[cur_node][i].second;  // 인접 정점까지 거리

            if (nxt_dist < dist[nxt_node])  // 지금 계산한 것이 기존 거리값보다 작다면
            {
                dist[nxt_node] = nxt_dist;  // 거리값 업데이트
                pq.push({ -nxt_dist, nxt_node });  // 우선순위 큐에 넣기
            }
        }
    }

    return dist;  // start 노드로부터 각 노드까지 최단거리를 담은 벡터 리턴
}

int main()
{
    const int N = 10;  // 노드의 개수가 10개라 가정.
    int E = 20;  // 간선의 개수가 20개라 가정.
    vector<pair<int, int>> graph[N];  // 그래프 형태 선언

    for (int i = 0; i < E; i++)
    {
        int from, to, cost;  // 간선의 시작점, 끝점, 가중치
        cin >> from >> to >> cost;
        graph[from].push_back({ to, cost });  // 무향 그래프라 가정하므로 시작점과 끝점 둘 다 벡터에 넣어야 함
        graph[to].push_back({ from, cost });
    }

    vector<int> dist = dijkstra(0, N, graph);
    
    cout << "끝점까지의 최단거리" << dist[N - 1] << endl;
    
    return 0;
}
```

# 복잡도
- 순차탐색(N = 노드의 수) : O(N^2)
- 우선순위 큐(E = 간선의 수, 노드의 수 V) : O(ElogV)
