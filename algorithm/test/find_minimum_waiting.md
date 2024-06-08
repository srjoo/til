# 문제
현대모비스는 우수한 SW 인재 채용을 위해 상시로 채용 설명회를 진행하고 있습니다. 채용 설명회에서는 채용과 관련된 상담을 원하는 참가자에게 멘토와 1:1로 상담할 수 있는 기회를 제공합니다. 채용 설명회에는 멘토 n명이 있으며, 1~k번으로 분류되는 상담 유형이 있습니다. 각 멘토는 k개의 상담 유형 중 하나만 담당할 수 있습니다. 멘토는 자신이 담당하는 유형의 상담만 가능하며, 다른 유형의 상담은 불가능합니다. 멘토는 동시에 참가자 한 명과만 상담 가능하며, 상담 시간은 정확히 참가자가 요청한 시간만큼 걸립니다.

참가자가 상담 요청을 하면 아래와 같은 규칙대로 상담을 진행합니다.

상담을 원하는 참가자가 상담 요청을 했을 때, 참가자의 상담 유형을 담당하는 멘토 중 상담 중이 아닌 멘토와 상담을 시작합니다.
만약 참가자의 상담 유형을 담당하는 멘토가 모두 상담 중이라면, 자신의 차례가 올 때까지 기다립니다. 참가자가 기다린 시간은 참가자가 상담 요청했을 때부터 멘토와 상담을 시작할 때까지의 시간입니다.
모든 멘토는 상담이 끝났을 때 자신의 상담 유형의 상담을 받기 위해 기다리고 있는 참가자가 있으면 즉시 상담을 시작합니다. 이때, 먼저 상담 요청한 참가자가 우선됩니다.
참가자의 상담 요청 정보가 주어질 때, 참가자가 상담을 요청했을 때부터 상담을 시작하기까지 기다린 시간의 합이 최소가 되도록 각 상담 유형별로 멘토 인원을 정하려 합니다. 단, 각 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.

예를 들어, 5명의 멘토가 있고 1~3번의 3가지 상담 유형이 있을 때 아래와 같은 참가자의 상담 요청이 있습니다.

참가자의 상담 요청

참가자 번호	시각	상담 시간	상담 유형
1번 참가자	10분	60분	1번 유형
2번 참가자	15분	100분	3번 유형
3번 참가자	20분	30분	1번 유형
4번 참가자	30분	50분	3번 유형
5번 참가자	50분	40분	1번 유형
6번 참가자	60분	30분	2번 유형
7번 참가자	65분	30분	1번 유형
8번 참가자	70분	100분	2번 유형
이때, 멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 25로 최소가 됩니다.

1번 유형	2번 유형	3번 유형
2명	1명	2명
1번 유형

1번 유형을 담당하는 멘토가 2명 있습니다.

1번 참가자가 상담 요청했을 때, 멘토#1과 10분~70분 동안 상담을 합니다.
3번 참가자가 상담 요청했을 때, 멘토#2와 20분~50분 동안 상담을 합니다.
5번 참가자가 상담 요청했을 때, 멘토#2와 50분~90분 동안 상담을 합니다.
7번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 5분 동안 기다리고 멘토#1과 70분~100분 동안 상담을 합니다.
2번 유형

2번 유형을 담당하는 멘토가 1명 있습니다.

6번 참가자가 상담 요청했을 때, 멘토와 60분~90분 동안 상담을 합니다.
8번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 6번 참가자의 상담이 끝날 때까지 20분 동안 기다리고 90분~190분 동안 상담을 합니다.
3번 유형

3번 유형을 담당하는 멘토가 2명 있습니다.

2번 참가자가 상담 요청했을 때, 멘토#1과 15분~115분 동안 상담을 합니다.
4번 참가자가 상담 요청했을 때, 멘토#2와 30분~80분 동안 상담을 합니다.
상담 유형의 수를 나타내는 정수 k, 멘토의 수를 나타내는 정수 n과 참가자의 상담 요청을 담은 2차원 정수 배열 reqs가 매개변수로 주어집니다. 멘토 인원을 적절히 배정했을 때 참가자들이 상담을 받기까지 기다린 시간을 모두 합한 값의 최솟값을 return 하도록 solution 함수를 완성해 주세요.

제한사항
1 ≤ k ≤ 5
k ≤ n ≤ 20
3 ≤ reqs의 길이 ≤ 300
reqs의 원소는 [a, b, c] 형태의 길이가 3인 정수 배열이며, c번 유형의 상담을 원하는 참가자가 a분에 b분 동안의 상담을 요청했음을 의미합니다.
1 ≤ a ≤ 1,000
1 ≤ b ≤ 100
1 ≤ c ≤ k
reqs는 a를 기준으로 오름차순으로 정렬되어 있습니다.
reqs 배열에서 a는 중복되지 않습니다. 즉, 참가자가 상담 요청한 시각은 모두 다릅니다.
입출력 예
k	n	reqs	result
3	5	[[10, 60, 1], [15, 100, 3], [20, 30, 1], [30, 50, 3], [50, 40, 1], [60, 30, 2], [65, 30, 1], [70, 100, 2]]	25
2	3	[[5, 55, 2], [10, 90, 2], [20, 40, 2], [50, 45, 2], [100, 50, 2]]	90
입출력 예 설명
입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

참가자의 상담 요청

참가자 번호	시각	상담 시간	상담 유형
1번 참가자	5분	55분	2번 유형
2번 참가자	10분	90분	2번 유형
3번 참가자	20분	40분	2번 유형
4번 참가자	50분	45분	2번 유형
5번 참가자	100분	50분	2번 유형
멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 90으로 최소가 됩니다.

1번 유형	2번 유형
1명	2명
1번 유형

1번 유형을 담당하는 멘토가 1명 있습니다. 1번 유형의 상담을 요청한 참가자가 없지만, 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.

2번 유형

2번 유형을 담당하는 멘토가 2명 있습니다.

1번 참가자가 상담 요청했을 때, 멘토#1과 5분~60분 동안 상담을 합니다.
2번 참가자가 상담 요청했을 때, 멘토#2와 10분~100분 동안 상담을 합니다.
3번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 40분 동안 기다리고 멘토#1과 60분~100분 동안 상담을 합니다. 1번 참가자의 상담이 끝났을 때 4번 참가자도 기다리고 있었지만, 먼저 상담 요청한 3번 참가자가 우선됩니다.
4번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 2번 참가자의 상담이 끝날 때까지 50분 동안 기다리고 멘토#2와 100분~145분 동안 상담을 합니다. 이때, 멘토#1과 상담할 수도 있지만 어느 멘토와 상담해도 상관없습니다.
5번 참가자가 상담 요청했을 때, 멘토#1과 100분~150분 동안 상담을 합니다.
따라서 90을 return 하면 됩니다.

# 테스트 예제
테스트 1
입력값 〉	3, 5, [[10, 60, 1], [15, 100, 3], [20, 30, 1], [30, 50, 3], [50, 40, 1], [60, 30, 2], [65, 30, 1], [70, 100, 2]]
기댓값 〉	25
실행 결과 〉	테스트를 통과하였습니다.
테스트 2
입력값 〉	2, 3, [[5, 55, 2], [10, 90, 2], [20, 40, 2], [50, 45, 2], [100, 50, 2]]
기댓값 〉	90
실행 결과 〉	테스트를 통과하였습니다.

# 제출
```java
import java.util.ArrayList;

class Solution {
    public int solution(int k, int n, int[][] reqs) {
        ArrayList<int[]>[] reqsList = new ArrayList[k];
        for(int i=0; i < k; i++) {
            reqsList[i] = new ArrayList<>();
        }
        for(int[] req : reqs) {
            reqsList[req[2]-1].add(req);
        }

        int freeWorker = n-k;
        Worker[] workers = new Worker[k];
        for(int i=0; i < k; i++) {
            workers[i] = new Worker(reqsList[i]);
        }

        int[][] waitTimeTable = new int[k][freeWorker+1];
        for(int i=0; i < k; i++) {
            for (int j = 0; j <= freeWorker; j++) {
                waitTimeTable[i][j] = workers[i].work(j);
                if(waitTimeTable[i][j] == 0) break;
            }
        }

        return calcTime(waitTimeTable, freeWorker, k-1, 0);
    }

    private int calcTime(int[][] waitTimeTable, int maximum, int maxDepth, int curDepth) {
        if(curDepth == maxDepth) {
            return waitTimeTable[curDepth][maximum];
        } else {
            int totalWait = Integer.MAX_VALUE;
            for(int i=0; i <= maximum; i++) {
                int wait = waitTimeTable[curDepth][i];
                totalWait = Math.min(wait + calcTime(waitTimeTable, maximum - i, maxDepth, curDepth + 1), totalWait);
                if(wait == 0) break;
            }
            return totalWait;
        }
    }

    class Worker {
        ArrayList<int[]> request;

        public Worker(ArrayList<int[]> request) {
            this.request = request;
        }

        public int work(int addWorker) {
            int[] worker = new int[1 + addWorker];
            int working = 0;
            int complete = 0;
            int time = 0;
            int waitTime = 0;

            while(complete < request.size()) {
                while (working < worker.length) {
                    if (time >= request.get(complete)[0]) {
                        waitTime += time - request.get(complete)[0];
                        worker[findIdleWorker(worker)] = request.get(complete)[1];
                        working++;
                        complete++;
                        if(complete == request.size()) {
                            break;
                        }
                    } else {
                        break;
                    }
                }

                if(complete < request.size()) {
                    int timePass = 0;
                    if(working >= worker.length) {
                        timePass = calcFinishTime(worker);
                    }
                    timePass = Math.max(timePass, Math.max(0, request.get(complete)[0]-time));
                    if(timePass <= 0) break;
                    time += timePass;
                    working -= doWork(worker, timePass);
                }
            }

            return waitTime;
        }

        private int findIdleWorker(int[] worker) {
            for(int i=0; i < worker.length; i++) {
                if(worker[i] == 0) return i;
            }
            return -1;
        }

        private int calcFinishTime(int[] worker) {
            int finishTime = Integer.MAX_VALUE;
            for(int i=0; i < worker.length; i++) {
                finishTime = Math.min(finishTime, worker[i]);
            }
            return finishTime;
        }

        private int doWork(int[] worker, int time) {
            int idle = 0;
            for(int i=0; i < worker.length; i++) {
                if(worker[i] > 0) {
                    worker[i] = Math.max(0, worker[i] - time);
                    if (worker[i] == 0) {
                        idle++;
                    }
                }
            }
            return idle;
        }
    }
}
```