# 유니온 파인드(Union-Find)
- 유니온 파인드는 그래프 알고리즘으로 두 노드가 같은 그래프에 속하는지 판별하는 알고리즘
- 노드를 합치는 Union연산과 노드의 루트 노드를 찾는 Find연산으로 이루어진다
> 서로소 집합(Disjoint-Set)으로도 불린다
> 트리 구조로 이루어진 자료구조 중 한가지로 생각되기도 한다

# 유니온 연산
- 각 노드가 속한 집합을 하나로 합치는 연산
- 노드 a,b가 a∈A, b∈B 일 때, union(a,b)는 AUB

# 파인드 연산
- 특정 노드 a에 관해 a가 속한 집합의 대표 노드를 반환하는 연산
- 노드 a가 a∈A일 때, find(a)는 A 집합의 대표 노드를 반환

# 구현
- 1~8까지의 노드가 있고, 1,2 노드가 연결되어있고, 4,5,6 노드가 연결되 있는 경우
```c++
#include<iostream>
using namespace std;
int parent[9]; // 위 그림에서 배열에 해당하는 것

int find(int x) {
	if (parent[x] == x) // 배열 인덱스와 값이 같다면 해당 값 리턴
		return x;
	return parent[x] = find(parent[x]); // 배열의 값을 인덱스로 갖는 값 리턴
}

void merge(int x, int y) {
	x = find(x);
	y = find(y); // 각각 find연산을 통해 루트 노드를 가짐
	if (x == y) return; // x와 y가 같다면 이미 연결되어있는 것
	parent[y] = x; // 배열의 y인덱스에 x값을 저장
}

bool isUnion(int x, int y) { // 두 노드가 연결되어있는지 판별하는 함수
	x = find(x);
	y = find(y);
	if (x == y)
		return true;
	return false;
}

int main() {
	for (int i = 1; i <= 8; i++) // 배열 초기화 과정
		parent[i] = i;

	merge(1, 2);
	merge(4, 5);
	merge(5, 6);
	cout << "2와 4는 같은 집합인가?\n" << isUnion(2, 4) << "\n"; // false
	merge(1, 5);
	cout << "2와 4는 같은 집합인가?\n" << isUnion(2, 4) << "\n"; // true
	return 0;
}
```