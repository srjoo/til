# 문제
x축과 y축으로 이루어진 2차원 직교 좌표계에 중심이 원점인 서로 다른 크기의 원이 두 개 주어집니다. 반지름을 나타내는 두 정수 r1, r2가 매개변수로 주어질 때, 두 원 사이의 공간에 x좌표와 y좌표가 모두 정수인 점의 개수를 return하도록 solution 함수를 완성해주세요.
※ 각 원 위의 점도 포함하여 셉니다.

제한 사항
1 ≤ r1 < r2 ≤ 1,000,000
입출력 예
r1	r2	result
2	3	20
입출력 예 설명
입출력 예 설명.png
그림과 같이 정수 쌍으로 이루어진 점은 총 20개 입니다.

# 테스트 예제
테스트 1
입력값 〉	2, 3
기댓값 〉	20
실행 결과 〉	테스트를 통과하였습니다.

# 제출
- 틀림, 두 원의 크기를 이용하여 제곱근으로 각 x 좌표당 점의 개수를 구해야하는데 그런 생각을 못해서 해답을 찾아봄

# 풀이
```java
class Solution {
    public long solution(int r1, int r2) {
        long answer = 0;
        // 1개 사분면만 계산
		// 1부터 시작하는 이유는 겹치는 부분의 계산을 피하기 위해서
        for(int i=1; i <= r2; i++){
		// r^2 = x^2 + y^2 ==> x^2 = r^2 - y^2 정수는 y1 <= ? <= y2 사이의 값이 된다
            double y2 = Math.sqrt(Math.pow(r2,2) - Math.pow(i,2));
            double y1 = Math.sqrt(Math.pow(r1,2) - Math.pow(i,2));
            answer += ((long)y2 - (long)Math.ceil(y1) + 1); // +1은 올림값의 보정값 또한 좌표값이 0이 나와도 그또한 정수기에 포함된다
        }
        answer *= 4;
        
        return answer;
    }
}
```