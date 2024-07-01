# class
- Mixin 기반 상속 지원
> Mixin 기반 상속이란 말은, 모든 클래스가 하나의 부모 클래스를 가지고 있지만 (최상위 클래스인 Object?를 제외한) 클래스의 바디는 다양한 클래스 계층에서 재사용 될 수 있음을 의미

# class member 사용
- .을 이용한다
- 객체가 널일수 있을 때는 ?.
```dart
// p가 non-null이라면, a의 값을 p의 y의 값으로 설정합니다.
var a = p?.y;
```

# constructor
- 생성자의 이름은 ClassName 또는 ClassName.identifier
```dart
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
```
- 생성자 이름에 선택적인 키워드인 new를 사용할 수 있다

# const constructor
- 상수 생성자를 사용하여 컴파일 타임 상수를 생성하고 싶다면, 생성자 이름 앞에 const를 사용
- 상수 생성자에서 동일한 인자를 가질 경우 같은 인스턴스이다
> 생성자를 const로 정의하고 모든 인스턴스 변수를 final로 선언
```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final double x, y;

  const ImmutablePoint(this.x, this.y);
}

void main() {
  var p = const ImmutablePoint(2, 2);
  
  var a = const ImmutablePoint(1, 1);
  var b = const ImmutablePoint(1, 1);

  assert(identical(a, b)); // 둘은 같은 인스턴스입니다!
}
```
- const를 선언 할 때 첫 번째 const를 제외하고 다른 const들은 생략 할 수 있다
```dart
// 불필요한 const 키워드가 많습니다.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};

// 상수 컨텍스트를 만들어주는 하나의 const만 사용하면 됩니다.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
```

# 객체 타입
- Object의 프로퍼티인 runtimeType을 사용
```dart
print('The type of a is ${a.runtimeType}');
```
> 객체의 타입을 테스트하려면, runtimeType 대신 타입 테스트 연산자를 사용 is Type 테스트가 object.runtimeType == Type 테스트 보다 더 안전

# final 인스턴스 변수
- 인스턴스 변수는 final로 선언할 수 있고, 단 한 번만 값 할당 가능
- 생성자 매개변수나, 생성자의 이니셜라이저 목록를 사용하여 초기화
```dart
class ProfileMark {
  final String name;
  final DateTime start = DateTime.now();

  ProfileMark(this.name);
  ProfileMark.unnamed() : name = '';
}
```
- 생성자 바디가 시작된 후 final 인스턴스 변수의 값을 할당하려면 factory 생성자 혹은 late final을 사용

# class 변수, 메서드
- 정적 변수 : static 변수, 정적 변수는 사용하기 전에는 초기화되지 않는다
```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}
```
- 정적 메서드 : this에 접근 할 수 없지만, 정적 변수에 대한 접근은 가능
```dart
import 'dart:math';

class Point {
  double x, y;
  Point(this.x, this.y);

  static double distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```
> 정적 메소드는 컴파일 타임 상수로 사용 할 수 있다