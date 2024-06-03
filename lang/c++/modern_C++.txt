# Modern C++
- C++11 부터를 Modern C++이라고 한다
> C스타일의 프로그래밍은 지양한다.(raw pointer의 사용, 배열, null로 종료되는 string 등)

# 특징
- RAII(Resource Acquisition Is Initialization) 원칙 강조
> 모든 자원은 객체에 의해 소유되어야 한다. 생성자에서 자원을 생성&전달받고, 소멸자에서 자원을 지운다. 자원을 소유한 객체가 범위를 벗어나면 그 자원을 OS로 반환하는 것을 보장한다
> RAII원칙을 위해 C++의 STL은 스마트 포인터 타입들을 제공한다
> std::unique_ptr, std::shared_ptr, std::weak_ptr
> 예제 : make_unique()를 호출하여 array 할당하는 클래스 Widget
```c++
#include <memory>
class Widget {
private:
	std::unique_ptr<int> data;
public:
	widget(const int size) {
		data = std::make_unique<int>(size);
	}
	void do_something() {}
};

void example() {
	widget w(1000000);
	// lifetime automatically tied to enclosing scope
	// construct w, including the w.data gadget member w.do_something();
} // automatic destruction and deallocation for w and w.data
```
> 스마트 포인터는 메모리 할당과 해제를 직접 다룬다.
> 기존 C스타일의 new와 delete는 unique_ptr 클래스에 의해 캡슐화되어 있다

- std::string
> C 스타일의 string은 버그를 많이 일으킨다. 대신 std::string을 사용하면 C-style string에서 발생하는 웬만한 버그는 모두 없앨 수 있다. 또한 std::string이 속도 측면에서 최적화도 잘되어 있으니까 이걸 사용하는 것을 추천한다
> 참고로, C++17부터는 std::string_view를 지원한다. std::string_view는 읽는 것만 가능한 변수이다

- STL의 컨테이너들은 모두 RAII원칙을 따르고 있다
> raw array를 사용하기보다, vector 사용을 권장
```c++
vector<string> apples;
apples.push_back("Granny Smith");
map<string, string> apple_color;
apple_color["Granny Smith"] = "Green";
```
> 성능 최적화가 필요시
> 1. array가 클래스 멤버일 때 타입을 넣어주는게 중요하다.
> 2. 순서가 상관 없을 땐 unordered_map을 사용해라. 각 요소마다 오버헤드가 적고 검색하는데 상수시간이 걸린다. 그치만 정화하고 효율저긍로 사용하는게 어려울 수 있다.
> 3. 정렬된 게 필요할 땐 vector를 사용해라
> C스타일의 array를 사용하지 마라. 기존 C스타일의 코드를 지원하고 싶으면 아래와 같은 API를 활용해라
```c++
f(vec.data(), vec.size())
```

- STL algorithms
자주 쓰이는 예시들
- fore_each : 컨테이너의 요소를 조회하는 알고리즘
- transform
- find_if : 검색 알고리즘
- sort, lower_bond : 정렬 알고리즘
> 참고로 비교연산자를 쓸 때는 엄격하게 '<'를 사용하고 할 수 있다면 lamda를 사용해라
```c++
auto comp = [](const widget& w1, const widget& w2) { return w1.widget() < w2.widget(); }
sort( v.begin(), v.end(), comp );
auto i = lower_bound( v.begin(), v.end(), comp );
```

- auto 키워드
> type 을 직접 명시하는 대신에 auto를 사용해라
> C++11 부터 변수, 함수, 템플릿 선언에 auto 키워드를 사용할 수 있다. auto는 컴파일러에게 객체의 타입을 추론할 수 있게 말해준다. 따라서 개발자가 직접 타입을 명시할 필요가 없다. auto는 특히 template이 중첩된 경우에 매우 유용하다.
```c++
map<int, list<string>>::iterator i = m.begin(); // C-style
auto i = m.begin(); // modern C++
```

> Ranged-based for 반복자
```c++
#include <iostream>
#include <vector>
int main() {
	std::vector<int> v {1,2,3};
	// C-style
	for(int i = 0; i < v.size(); ++i) {
		std::cout << v[i];
	}
	// Modern C++
	for(auto& num : v) {
		std::cout << num;
	}
}
```

- macros 대신 constexpr 표현
> C와 C++에서의 매크로는 컴파일 전에 전처리기에 의해 처리되는 토큰이다. 각 매크로는 파일이 컴파일 되기 전에 미리 정의해놓은 값이나 표현으로 대체된다. 보통 매크로는 C-style programming에서 컴파일 타임 상수를 정의하기 위해 사용된다. 그러나 매크로는 에러를 발생시키기 쉽고 디버깅하기 어렵다. Modern C++ 에서는 컴파일타임 상수를 위해 constexpr을 사용한다.
```c++
#define SIZE 10 // C-style
constexpr int size = 10; // modern C++
```

- 일관된 초기화
> modern C++ 에서는 어떤 타입이든지 중괄호를 이용해 초기화할 수 있다. 이 형태의 초기화는 특히 arrays, vector 등의 컨테이너를 초기화할 때 편리
```c++
#include <vector>

struct S {
	std::string name;
	float num;
	S(std::string s, float f) : name(s), num(f) {}
};
	
int main() {
	// C-style initialization
	std::vector<S> v;
	S s1("Norah", 2.7);
	S s2("Frank", 3.5);
	S s3("Jeri", 85.9);
	v.push_back(s1);
	v.push_back(s2);
	v.push_back(s3);
	
	// Modern C++
	std::vector<S> v2 {s1, s2, s3};
	// or ...
	std::vector<S> v3{
		{"Norah", 2.7},
		{"Frank", 3.5},
		{"Jeri", 85.9}
	};
}
```

- move
> 모던 c++에서는 move 시멘틱을 제공한다
> move는 불필요한 메모리 복사를 없앨 수 있다
> C++의 초기 버전에서는 특정한 상황에서의 복사를 피할 수 없었다. move 오퍼레이션은 복사없이 resource의 소유권을 한 오브젝트에서 다른 오브젝트로 옮길 수 있다
> 어떤 클래스들은 힙 메모리, 파일 핸들러와 같은 자원을 소유한다
> 이렇게 자원을 소유하고 있는 클래스를 선언할 때, move 생성자와 move 할당 오퍼레이션을 선언하면 된다
> STL 컨테이너 타입들은 정의될 때 오브젝트에서 move 생성자를 호출한다.

- Lambda 표현
> C스타일 프로그래밍에서 function pointer를 사용하여 함수에서 다른 함수로 전달될 수 있었다
> Function pointers은 유지 보수 및 코드 읽기성을 저해한다
> 또한 function pointer는 안전한 타입이 아니다
> modern C++ 은 오퍼레이터()로 재정의된 함수 오브젝트와 클래스들을 제공
> 이것들은 함수처럼 불릴 수 있다
> 함수 오브젝트를 생성하는 가장 간편한 방법은 인라인 lambda 표현을 사용하는 것이다.
```c++
std::vector<int> v {1,2,3,4,5};
int x = 2;
int y = 4;
auto result = find_if(begin(v), end(v), [=](int i) {
	return i > x && i <y; }
);
```

- std::atomic
> inter-thread communication을 위해 atd::atomic이나 관련된 타입들을 사용해라