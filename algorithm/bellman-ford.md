# 벨만-포드 알고리즘(Bellman-Ford Algorithm)
- 벨만-포드 알고리즘은 한 노드에서 다른 노드까지의 최단 거리를 구하는 알고리즘
- 간선의 가중치가 음수일 때도 최단 거리를 구할 수 있다!(중요, 다익스트라를 못 쓸때 쓸 수 있다!)

# 다익스트라 알고리즘과의 차이점
- 매 반복마다 모든 간선을 확인한다
> 다익스트라의 경우 방문하지 않은 노드 중에서 최단 거리가 가장 가까운 노드만을 방문
- 음수일 때도 최적 해를 구할 수 있다
- 시간 복잡도가 다익스트라 보다 느리다 : O(VE)

# 과정
1. 출발 노드를 설정
2. 최단 거리 테이블을 초기화
3. 다음의 과정을 (V(=정점) - 1)번 반복
>> (가) 모든 간선 E개를 하나씩 확인
>> (나) 각 간선을 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신
>> 음수 간선 순환이 발생하는지 체크하고 싶다면 3번 과정을 한 번 더 수행(최단 거리 테이블이 갱신된다면 음수 간선 순환 존재)
>> 음의 간선 순환이 존재할 경우 최단 거리가 -무한대가 되므로 해를 구할 수 없다

# 구현
```python
import sys

input = sys.stdin.readline
INF = int(1e9)

# 노드의 개수, 간선의 개수를 입력
v, e = map(int, input().split())
# 모든 간선에 대한 정보를 담는 리스트 만들기
edges = []
# 최단 거리 테이블을 모두 무한으로 초기화
distance = [INF] * (v + 1)

# 모든 간선의 정보 입력
for _ in range(e):
    a, b, c = map(int, input().split())
    # a번 노드에서 b번 노드로 가는 비용이 c라는 의미 (a -> b 의 비용이 c)
    edges.append((a, b, c))


def bellman_ford(start):
    # 시작 노드에 대해서 초기화
    distance[start] = 0
    # 전체 v - 1번의 라운드(round)를 반복
    for i in range(v):
        # 매 반복마다 '모든 간선'을 확인한다.
        for j in range(e):
            cur_node = edges[j][0]
            next_node = edges[j][1]
            edge_cost = edges[j][2]
            # 현재 간선을 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우
            if distance[cur_node] != INF and distance[next_node] > distance[cur_node] + edge_cost:
                distance[next_node] = distance[cur_node] + edge_cost
                # v번째 라운드에서도 값이 갱신된다면 음수 순환이 존재
                if i == v - 1:
                    return True

    return False


# 벨만 포드 알고리즘 수행
negative_cycle = bellman_ford(1)

# 음수 순환이 존재하면 -1 출력
if negative_cycle:
    print("-1")
else:
    # 1번 노드를 제외한 다른 모든 노드로 가기 위한 최단 거리를 출력
    for i in range(2, v + 1):
        # 도달할 수 없는 경우, -1 출력
        if distance[i] == INF:
            print("-1")
        # 도달할 수 있으면 거리 출력
        else:
            print(distance[i])
```

# 복잡도
- V번 반복에 대해서 해당 정점과 연결되어 있는 모든 간선(E)을 탐색
- O(V*E) = O(VE)