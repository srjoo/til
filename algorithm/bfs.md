# 너비 우선 탐색(BFS, Breadth-First Search)
- 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법
- 깊게 탐색하기 전 넓게 탐색하는 법
- 깊이 우선 탐색(DFS) 보다 복잡

# 특징
- 직관적이지 않다
- 재귀적이지 않다
- 그래프 탐색의 경우 반드시 어떤 노드를 방문했는지 기록해야함
- 선입선출(FIFO, 큐)로 탐색
- Prim, Dijkstra 알고리즘과 비슷하다

# 주로 사용하는 문제
- 두 노드 사이의 최단 경로 혹은 임의의 경로를 찾기
- EX) A에서 B까지 가는 길을 찾는다고 할 때, A와 인접한 부분 부터 찾기 시작한다

# 과정
1. 시작 노드를 큐에 넣는다
2. 큐에서 노드를 꺼내 이동한다
3. 노드에서 이어지는 다른 노드를 모두 큐에 넣는다
4. 2~3을 반복

# 구현
- 의사코드
```java

/*
 * 의사코드
void search(Node root) {
  Queue queue = new Queue();
  root.marked = true; // (방문한 노드 체크)
  queue.enqueue(root); // 1-1. 큐의 끝에 추가

  // 3. 큐가 소진될 때까지 계속한다.
  while (!queue.isEmpty()) {
    Node r = queue.dequeue(); // 큐의 앞에서 노드 추출
    visit(r); // 2-1. 큐에서 추출한 노드 방문
    // 2-2. 큐에서 꺼낸 노드와 인접한 노드들을 모두 차례로 방문한다.
    foreach (Node n in r.adjacent) {
      if (n.marked == false) {
        n.marked = true; // (방문한 노드 체크)
        queue.enqueue(n); // 2-3. 큐의 끝에 추가
      }
    }
  }
}
*/

import java.io.*;
import java.util.*;

/* 인접 리스트를 이용한 방향성 있는 그래프 클래스 */
class Graph {
  private int V; // 노드의 개수
  private LinkedList<Integer> adj[]; // 인접 리스트

  /** 생성자 */
  Graph(int v) {
    V = v;
    adj = new LinkedList[v];
    for (int i=0; i<v; ++i) // 인접 리스트 초기화
      adj[i] = new LinkedList();
  }

  /** 노드를 연결 v->w */
  void addEdge(int v, int w) { adj[v].add(w); }

  /** s를 시작 노드으로 한 BFS로 탐색하면서 탐색한 노드들을 출력 */
  void BFS(int s) {
    // 노드의 방문 여부 판단 (초깃값: false)
    boolean visited[] = new boolean[V];
    // BFS 구현을 위한 큐(Queue) 생성
    LinkedList<Integer> queue = new LinkedList<Integer>();

    // 현재 노드를 방문한 것으로 표시하고 큐에 삽입(enqueue)
    visited[s] = true;
    queue.add(s);

    // 큐(Queue)가 빌 때까지 반복
    while (queue.size() != 0) {
      // 방문한 노드를 큐에서 추출(dequeue)하고 값을 출력
      s = queue.poll();
      System.out.print(s + " ");

      // 방문한 노드와 인접한 모든 노드를 가져온다.
      Iterator<Integer> i = adj[s].listIterator();
      while (i.hasNext()) {
        int n = i.next();
        // 방문하지 않은 노드면 방문한 것으로 표시하고 큐에 삽입(enqueue)
        if (!visited[n]) {
          visited[n] = true;
          queue.add(n);
        }
      }
    }
  }
}
```

# 시간복잡도
- 인접 리스트로 표현된 그래프: O(N+E)
- 인접 행렬로 표현된 그래프: O(N^2)
- 깊이 우선 탐색(DFS)과 마찬가지로 그래프 내에 적은 숫자의 간선만을 가지는 희소 그래프(Sparse Graph) 의 경우 인접 행렬보다 인접 리스트를 사용하는 것이 유리