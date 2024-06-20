# 동적계획법(Dynamic Programming)
- 하나의 큰 문제를 여러 개의 작은 문제로 나누어서 그 결과를 저장하여 다시 큰 문제를 해결할 때 사용

# 사용 조건
- DP를 사용하기 위해서는 2가지 조건을 만족해야된다
1. Overlapping Subproblems(겹치는 부분 문제) : 동일한 작은 문제들이 반복하여 나타나는 경우
2. Optimal Substructure(최적 부분 구조) : 부분 문제의 최적 결과 값을 사용해 전체 문제의 최적 결과를 낼 수 있는 경우

# 사용하기
1. DP로 풀 수 있는 문제인지 확인
2. 문제의 변수 파악 : 문제 내 변수의 갯수를 알아내야함
3. 변수간 관계식 만들기(점화식)(피보나치의 경우 f(n) = f(n-1) + f(n-2)
4. 메모하기 : 변수의 값에 따른 결과를 저장
5. 기저 상태 파악 : 가장 작은 문제의 상태를 알아낸다(피보나치의 경우 f(0) = 0, f(1) = 1 케이스)
6. 구현 : Bottom-Up(반복) 혹은 Top-Down(재귀)로 구현할 수 있다

# Bottom-Up
- 상태 저장을 위해 dp라는 배열을 만들었다면, dp[0]이 기저 상태이고 dp[n]이 목표 상태이다. dp[0]에서 시작하여 dp[n]까지 그 값을 전이시켜 재활용한다

# Top-Down
- dp[n]에서 호출을 시작하며 dp[0]의 상태까지 내려간 다음 재귀를 통해 재활용한다

# 구현
- 피보나치 수열
```java
packge com.test;

public class Fibonacci{
    // DP 를 사용 시 작은 문제의 결과값을 저장하는 배열
    // Top-down, Bottom-up 별개로 생성하였음(큰 의미는 없음)
    static int[] topDown_memo; 
    static int[] bottomup_table;
    public static void main(String[] args){
        int n = 30;
        topDown_memo = new int[n+1];
        bottomup_table = new int[n+1];
        
        long startTime = System.currentTimeMillis();
        System.out.println(naiveRecursion(n));
        long endTime = System.currentTimeMillis();
        System.out.println("일반 재귀 소요 시간 : " + (endTime - startTime));
        
        System.out.println();
        
        startTime = System.currentTimeMillis();
        System.out.println(topDown(n));
        endTime = System.currentTimeMillis();
        System.out.println("Top-Down DP 소요 시간 : " + (endTime - startTime));
        
        System.out.println();
        
        startTime = System.currentTimeMillis();
        System.out.println(bottomUp(n));
        endTime = System.currentTimeMillis();
        System.out.println("Bottom-Up DP 소요 시간 : " + (endTime - startTime));
    }
    
    // 단순 재귀를 통해 Fibonacci를 구하는 경우
    // 동일한 계산을 반복하여 비효율적으로 처리가 수행됨
    public static int naiveRecursion(int n){
        if(n <= 1){
            return n;
        }
        return naiveRecursion(n-1) + naiveRecursion(n-2);
    }
    
    // DP Top-Down을 사용해 Fibonacci를 구하는 경우
    public static int topDown(int n){
        // 기저 상태 도달 시, 0, 1로 초기화
        if(n < 2) return topDown_memo[n] = n;
        
        // 메모에 계산된 값이 있으면 바로 반환!
        if(topDown_memo[n] > 0) return topDown_memo[n];
        
        // 재귀를 사용하고 있음!
        topDown_memo[n] = topDown(n-1) + topDown(n-2);
        
        return topDown_memo[n];
    }
    
    // DP Bottom-Up을 사용해 Fibonacci를 구하는 경우
    public static int bottomUp(int n){
        // 기저 상태의 경우 사전에 미리 저장
        bottomup_table[0] = 0; bottomup_table[1] = 1;
        
        // 반복문을 사용하고 있음!
        for(int i=2; i<=n; i++){
            // Table을 채워나감!
            bottomup_table[i] = bottomup_table[i-1] + bottomup_table[i-2];
        }
        return bottomup_table[n];
    }
}

/*
결과
832040
일반 재귀 소요 시간 : 9

832040
Top-Down DP 소요 시간 : 0

832040
Bottom-Up DP 소요 시간 : 0
*/
```

# 분할 정복과의 차이점
- 주어진 문제를 작게 쪼개서 하위 문제로 해결하고 연계적으로 큰 문제를 해결한다는 점은 같다
- 하지만 동일한 중복이 일어나면 DP를 쓴다
> 예를 들어 병합 정렬의 경우 분할된 부분이 모두 독립적이다. 따라서 DP로 해결이 불가능하다.