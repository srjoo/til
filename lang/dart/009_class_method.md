# 인스턴스 메서드
- 객체의 인스턴스 메서드는 인스턴스 변수와 this에 접근 가능

# 연산자 오버로딩
- ReturnType operator OPERATOR(args)
```dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  @override
  bool operator ==(Object other) =>
      other is Vector && x == other.x && y == other.y;

  @override
  int get hashCode => Object.hash(x, y);
}
```

# Getter, setter
- 객체의 프로퍼티를 읽고(get) 쓰는(set) 함수
- 모든 인스턴스 변수는 암묵적으로 getter와 setter를 가진다
- get과 set 키워드를 사용하여 별도 getter와 setter를 구현 가능
- getter : Type get Name
- setter : set Name(args)
```dart
class Rectangle {
  double left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // right, bottom 이라는 두 개의 계산된 프로퍼티 정의.
  double get right => left + width;
  set right(double value) => left = value - width;
  double get bottom => top + height;
  set bottom(double value) => top = value - height;
}
```

# 추상 메서드
- 인터페이스만 구현한 상태로 나머지 부분은 다른 클래스들에게 맡기는 것
- 추상 메서드는 오직 추상 클래스에 존재할 수 있다
```dart
abstract class Doer {
  // 인스턴스 변수와 메서드 정의 ...

  void doSomething(); // 추상 메서드 정의.
}
```