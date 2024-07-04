# 열거형(Enum)
- 정해진 수의 상수 값을 가지는 특별한 종류의 클래스
- 기본 선언
```dart
enum Color { red, green, blue, } //  trailing commas 사용 가능
```

# 복잡한 Enum
- 필드, 메서드, 상수 생성자같이 수가 정해져 있는 상수 인스턴스가 있는 클래스를 선언하는 데 enum을 사용
- mixins으로 추가되는 변수들까지 모든 인스턴스 변수들은 final 선언
- const 생성자로 선언해야함
- Factory 생성자는 고정된 enum 인스턴스 중 하나만을 반환 가능
- Enum이 자동으로 확장되므로 다른 클래스들은 Enum으로 확장될 수 없다
- index, hashCode, 항등 연산자 ==는 재정의할 수 없다
- value로 명명된 멤버는 enum에 선언될 수 없다(자동으로 생성된 정적 value getter와 충돌)
- Enum의 모든 인스턴스들은 선언의 처음 부분에 선언되어야함
```dart
enum Vehicle implements Comparable<Vehicle> {
  car(tires: 4, passengers: 5, carbonPerKilometer: 400),
  bus(tires: 6, passengers: 50, carbonPerKilometer: 800),
  bicycle(tires: 2, passengers: 1, carbonPerKilometer: 0);

  const Vehicle({
    required this.tires,
    required this.passengers,
    required this.carbonPerKilometer,
  });

  final int tires;
  final int passengers;
  final int carbonPerKilometer;

  int get carbonFootprint => (carbonPerKilometer / passengers).round();

  bool get isTwoWheeled => this == Vehicle.bicycle;

  @override
  int compareTo(Vehicle other) => carbonFootprint - other.carbonFootprint;
}
```

# Enum 사용
- 정적 변수에 접근하는 것 처럼 열거 값에 접근
- Enum의 각 값들은 index getter 메서드가 있습니다. 이 메서드는 0을 기준으로 인덱스된 위치 값을 반환합니다. 예를 들어, 첫 번째 값은 index 0을 가지고 두 번째 값은 index 1을 가진다
```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```
- Enum의 모든 값을 보고 싶으면 values 상수 사용
```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```