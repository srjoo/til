# 문제
n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

제한사항
노드의 개수 n은 2 이상 20,000 이하입니다.
간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.
입출력 예
n	vertex	return
6	[[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]]	3
입출력 예 설명
예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

1.png

# 테스트 예제
테스트 1
입력값 〉	6, [[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]]
기댓값 〉	3
실행 결과 〉	테스트를 통과하였습니다.

# 제출
```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public int solution(int n, int[][] edge) {
        HashSet<Integer>[] lineSet = new HashSet[n];
        for (int[] ints : edge) {
            int from = ints[0];
            int to = ints[1];
            HashSet set = lineSet[from-1];
            if (set == null) {
                set = new HashSet<>();
                lineSet[from-1] = set;
            }
            set.add(to);

            set = lineSet[to-1];
            if (set == null) {
                set = new HashSet<>();
                lineSet[to-1] = set;
            }
            set.add(from);
        }

        int[] visited = new int[n];
        Arrays.fill(visited, -1);
        find(1, visited, lineSet);
        int max = 0;
        int maxCount = 0;
        for(int i : visited) {
            if(i == max) {
                maxCount++;
            } else if(i > max) {
                max = i;
                maxCount = 1;
            }
        }

        return maxCount;
    }

    public void find(int node, int[] visited, HashSet<Integer>[] lineSet) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(node);
        visited[node-1] = 0;
        while (!queue.isEmpty()) {
            bfs(queue, queue.poll(), lineSet, visited);
        }
    }

    public void bfs(Queue<Integer> queue, int node, HashSet<Integer>[] lineSet, int[] visited) {
        HashSet<Integer> set = lineSet[node-1];
        if(set != null) {
            int distance = visited[node-1];
            for(Integer i : set) {
                if(visited[i-1] == -1) {
                    visited[i-1] = distance + 1;
                    queue.add(i);
                }
            }
        }
    }
}
```