# 시간복잡도(Time Complexity)
- 알고리즘의 성능을 설명
- 명령문의 실행 빈도수를 계산

# Big-O
- 알고리즘 성능을 수학적으로 표시해주는 표기법
- 데이터나 사용자 증가율에 따른 알고리즘 성능을 예측하는 것이 목표이므로, 중요하지 않은 부분인 상수와 같은 숫자는 모두 제거
- 함수의 최고차항을 남기고 차수를 지워 O(...)와 같이 표기, 예를 들면 f(x) = 2x^2 + 3x + 5 = O(x^2)
- 불필요한 연산을 제거하여 알고리즘 분석을 쉽게 할 목적으로 사용
- 최악의 경우를 기준으로 삼는다
- 시간 복잡도 : 입력되는 n의 크기에 따라 실행되는 조작의 수
- 공간 복잡도 : 알고리즘이 실행될 때 사용하는 메모리 양

# Big-O 표기법 종류
- O(1) : Constant Time (상수), 입력 데이터 크기에 상관없이 언제나 일정한 시간이 걸리는 알고리즘
- O(n) : Linear Time (선형), 입력 데이터의 크기에 비례해서 처리 시간이 걸리는 알고리즘
- O(log n) : 한 번 반복할 때마다, 처리해야하는 값이 절반씩 사라지는 알고리즘의 시간 복잡도, O(log n)은 순차 검색 O(n)보다도 훨씬 빠르며, Big-O 표기법 중에서 O(1) 다음으로 빠른 시간 복잡도를 가지는 알고리즘
- O(n^2) : 입력 데이터의 크기의 제곱만큼 비례하여 처리 시간이 증가하는 알고리즘, 크기가 커질수록 처리 시간이 기하급수적으로 증가
- O(2^n) : Exponential Time, 걸리는 시간이 배로 증가하기 때문에 Big-O 표기법 중 가장 느린 시간 복잡도

# Ω(Omega) 표기법
- 최선의 경우(Best Case)를 표기할 때 사용된다

# Θ(Theta) 표기법
- 평균적인 경우를 표기할 때 사용된다
- Θ 표기법을 계산하여 사용하는 것이 가장 이상적이지만, 계산이 복잡하기 때문에 보통은 Big-O 표기법을 사용
	