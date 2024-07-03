# 형식 매개변수 초기화
- 매개변수 초기화는 초기화 되어야만 하거나 기본 값이 주어져야하는 non-nullable 또는 final 인스턴스 변수에만 사용이 가능
```dart
class Point {
  final double x;
  final double y;

  // 생성자 바디가 실행되기 전에 x와 y 인스턴스 변수 설정.
  Point(this.x, this.y);
}
```
> Initializing formal로 주어진 변수는 초기화 리스트 범위에서 암묵적으로 final

# 디폴트 생성자
- 생성자를 선언하지 않았다면, 디폴트 생성자
- 인자가 없고 인자가 없는 부모 클래스 생성자를 호출한다

# 생성자는 상속되지 않습니다
- 자식 클래스는 부모 클래스로 부터 생성자를 상속받지 않습니다

# 명명된 생성자
- 부모 클래스의 생성자는 자식 클래스로 상속되지 않는다는 것을 꼭 기억하세요. 자식 클래스에서 부모 클래스와 같은 명명된 생성자를 사용하고 싶다면, 자식 클래스에서도 똑같이 구현
```dart
const double xOrigin = 0;
const double yOrigin = 0;

class Point {
  final double x;
  final double y;

  Point(this.x, this.y);

  // 명명된 생성자
  Point.origin()
      : x = xOrigin,
        y = yOrigin;
}
```

# 부모 클래스의 Non-default 생성자 호출
- 자식 클래스의 생성자는 부모 클래스의 이름이 없고(unnamed), 인수가 없는(no-argument) 생성자를 디폴트로 호출
- 생성자가 실행되기 전에 부모 클래스의 생성자로 전해지는 인자가 평가되기 때문에 인자는 함수 호출처럼 표현식이 될 수 있습니다
```dart
class Employee extends Person {
  Employee() : super.fromJson(fetchDefaultData());
  // ···
}
```
> 부모 클래스의 생성자로 전달되는 인수는 this에 접근할 수 없습니다. 예를 들면, 인자는 정적 메서드를 호출할 수 있지만, 인스턴스 메서드는 불가능

# Super 매개변수
- 수동으로 부모 클래스의 생성자 매개변수를 넘겨주는 것을 피하고 싶다면, super 이니셜라이저 매개변수를 부모 클래스의 생성자로 넘겨주면 됩니다
```dart
class Vector2d {
  final double x;
  final double y;

  Vector2d(this.x, this.y);
}

class Vector3d extends Vector2d {
  final double z;

  // 매개변수 x와 y를 디폴트 super 생성자로 넘겨줍니다:
  // Vector3d(final double x, final double y, this.z) : super(x, y);
  Vector3d(super.x, super.y, this.z);
}
```
- Super 생성자 호출이 positional 인자를 가지고 있다면, Super 이니셜라이저 매개변수는 positional이 될 수 없지만 명명된 매개변수는 언제나 가능
```dart
class Vector2d {
  // ...

  Vector2d.named({required this.x, required this.y});
}

class Vector3d extends Vector2d {
  // ...

  // 매개변수 y를 명명된 super 생성자로 넘겨줍니다:
  // Vector3d.yzPlane({required double y, required this.z})
  //       : super.named(x: 0, y: y);
  Vector3d.yzPlane({required super.y, required this.z}) : super.named(x: 0);
}
```

# 이니셜라이저 리스트
- 자 바디가 실행되기 전에 부모 클래스의 생성자를 호출할 뿐만 아니라 인스턴스 변수를 초기화할 수도 있다
```dart
// 이니셜라이저 리스트는 생성자 바디가 실행되기 전에 인스턴스 변수를 설정합니다.
Point.fromJson(Map<String, double> json)
    : x = json['x']!,
      y = json['y']! {
  print('In Point.fromJson(): ($x, $y)');
}
```
- final 필드를 설정할 때 유용
```dart
import 'dart:math';

class Point {
  final double x;
  final double y;
  final double distanceFromOrigin;

  Point(double x, double y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

void main() {
  var p = Point(2, 3);
  print(p.distanceFromOrigin);
}
```

# 리다이렉팅 생성자
- 리다이렉팅 생성자의 바디는 비어있고 콜론 (:) 뒤에 나오며 클래스 이름 대신 this를 사용한 생성자 호출로 구성
```dart
class Point {
  double x, y;

  // 클래스의 메인 생성자.
  Point(this.x, this.y);

  // 메인 생성자로 리디렉트.
  Point.alongXAxis(double x) : this(x, 0);
}
```

# 상수 생성자
- 어떤 클래스가 절대 바뀌지 않는 객체를 생성한다면, 이 객체를 컴파일 타임 상수로 만들 수 있습니다. 생성자를 const로 정의하고 모든 인스턴스 변수를 final로 선언하면 됩니다.
```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final double x, y;

  const ImmutablePoint(this.x, this.y);
}
```

# Factory 생성자
- 항상 클래스의 새로운 인스턴스를 생성하지 않는 생성자를 구현하고 싶다면, factory 키워드를 사용
```dart
class Logger {
  final String name;
  bool mute = false;

  // _cache는 맨 앞의 _ 덕분에 library-private입니다.
  static final Map<String, Logger> _cache = <String, Logger>{};

  factory Logger(String name) {
    return _cache.putIfAbsent(name, () => Logger._internal(name));
  }

  factory Logger.fromJson(Map<String, Object> json) {
    return Logger(json['name'].toString());
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```
> Factory 생성자는 this에 접근할 수 없습니다