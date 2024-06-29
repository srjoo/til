# 함수
- Function 이라는 타입을 가지는 객체로 존재
- 리턴값 함수이름(인자타입 인자이름, ...) { 내용 }
- C++, Java 스타일인듯
```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```
- 리턴값을 생략해도 동작은 하지만 좋지 않다

# 약칭(shorthand) 문법
- 하나의 표현식만 있을 때는 => 로 표현 가능
```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```
> 문(statement)이 아닌 오직 식(expression)만이 화살표 (=>)와 세미콜론 (;) 사이에 올 수 있습니다. 예를 들어, if 문은 불가능하지만, 조건식은 가능합니다

# 매개변수
- trailing commas 사용 가능
```dart
void printUserDetails({
  required String name,
  required int age,
  String? city, // 마지막 인자에 ,가 있어도 OK
}) {
  print('Name: $name, Age: $age, City: $city');
}
```
- 필수 위치 매개변수 : 함수 호출 시 반드시 순서대로 전달해야 하는 매개변수
```dart
void printCoordinates(int x, int y) {
  print('Coordinates: ($x, $y)');
}
```
- 명명된 매개변수 : 함수 호출 시 인자의 이름을 명시적으로 지정할 수 있는 매개변수입니다. 명명된 매개변수는 선택적일 수 있으며, required 키워드를 사용하여 필수로 만들 수 있다
```dart
void printUserInfo({required String name, int age = 0}) {
  print('Name: $name, Age: $age');
}
```
- 선택적 위치 매개변수 : 선택적 위치 매개변수는 함수 호출 시 생략할 수 있는 매개변수, 대괄호([])로 감싸서 정의
```dart
void printNumbers(int a, [int b = 0, int c = 0]) {
  print('a: $a, b: $b, c: $c');
}
```
> 필수 위치 매개변수는 항상 명명된 매개변수 또는 선택적 위치 매개변수보다 앞에 와야 합니다. 두 가지 유형의 선택적 매개변수(명명된 매개변수와 선택적 위치 매개변수)를 동시에 사용할 수는 없습니다.

# main 함수
- void main() 혹은 void main(List<String>)

# 익명 함수(Anonymous Functions)
- ([Type] param1, [Type] param2, ...) {함수 본문}
```dart
void main() {
  const list = ['apples', 'bananas', 'oranges'];
  list.map((item) {
    return item.toUpperCase();
  }).forEach((item) {
    print('$item: ${item.length}');
  });
}
```
- 화살표 함수 (Arrow Functions) : 익명 함수가 하나의 표현식이나 반환문을 가질 때, 화살표 표기법을 사용 가능
```dart
void main() {
  const list = ['apples', 'bananas', 'oranges'];
  list
    .map((item) => item.toUpperCase())
    .forEach((item) => print('$item: ${item.length}'));
}
```
- 클로저(Closures) : 자신이 생성될 때의 변수들을 기억하고, 나중에 참조할 수 있는 함수
```dart
void main() {
  var counter = 0;

  var increment = () {
    counter++;
    print(counter);
  };

  increment(); // 1 출력
  increment(); // 2 출력
}
```

# 함수의 동일 판정
- 다른 인스턴스의 인스턴스 메서드는 동일하지 않다
```dart
void foo() {} // 최상위 함수

class A {
  static void bar() {} // 정적 메서드
  void baz() {} // 인스턴스 메서드
}

void main() {
  Function x;

  // 최상위 함수를 비교.
  x = foo;
  assert(foo == x);

  // 정적 메서드를 비교.
  x = A.bar;
  assert(A.bar == x);

  // 인스턴스 메서드를 비교.
  var v = A(); // A의 인스턴스 #1
  var w = A(); // B의 인스턴스 #2
  var y = w;
  x = w.baz;

  // 두 클로저들은 같은 인스턴스 (#2)를 참조하므로 동일합니다.
  assert(y.baz == x);

  // 두 클로저들은 다른 인스턴스를 참조하므로 동일하지 않습니다.
  assert(v.baz != w.baz);
}
```

# 리턴값
- 모든 함수는 값을 반환합니다. 반환 값이 명시되어 있지 않으면, return null;이 암묵적으로 함수의 바디에 추가

# 제너레이터(Generators)
- 이터러블(Iterable) 객체를 쉽게 생성할 수 있는 강력한 도구
- 동기 제네레이터 : 동기 제너레이터는 이터러블(Iterable)을 반환합니다. sync* 키워드를 사용하여 정의하며, yield 키워드를 사용하여 값을 하나씩 반환
```dart
Iterable<int> generateNumbers(int n) sync* {
  for (int i = 0; i < n; i++) {
    yield i;
  }
}

void main() {
  final numbers = generateNumbers(5);
  for (var number in numbers) {
    print(number); // 0, 1, 2, 3, 4 출력
  }
}
```
- 비동기 제너레이터 : 스트림(Stream)을 반환합니다. async* 키워드를 사용하여 정의하며, yield 키워드를 사용하여 값을 하나씩 반환합니다. await 키워드를 사용하여 비동기 작업을 수행할 수 있습니다
```dart
Stream<int> generateNumbersAsync(int n) async* {
  for (int i = 0; i < n; i++) {
    await Future.delayed(Duration(seconds: 1)); // 1초 지연
    yield i;
  }
}

void main() async {
  final stream = generateNumbersAsync(5);
  await for (var number in stream) {
    print(number); // 0, 1, 2, 3, 4 출력 (각 숫자 사이에 1초 지연)
  }
}
```