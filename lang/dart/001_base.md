# Dart 기초 문법
- 문장의 끝에 세미콜론 필요
- java하고 비슷한 느낌

# Hello World
```dart
void main() {
  print('Hello, World!');
}
```

# if
```dart
if (year >= 2001) {
  print('21st century');
} else if (year >= 1901) {
  print('20th century');
}
```

# for
```dart
for (final object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}
```

# while
```dart
while (year < 2016) {
  year += 1;
}
```

# function
```dart
int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
```

# 익명 function
```dart
(name) => name.contains('turn')
```

# class
```
class Spacecraft {
  String name;
  DateTime? launchDate;

  // Read-only non-final 프로퍼티
  int? get launchYear => launchDate?.year;

  // 멤버 할당에 신택틱 슈가를 사용한 생성자
  Spacecraft(this.name, this.launchDate) {
    // 여기에 초기화 코드가 이어집니다.
  }

  // 디폴트 생성자로 포워드하는 Named 생성자.
  Spacecraft.unlaunched(String name) : this(name, null);

  // 메서드.
  void describe() {
    print('Spacecraft: $name');
    // Getter에는 타입 프로모션이 작동하지 않습니다.
    var launchDate = this.launchDate;
    if (launchDate != null) {
      int years = DateTime.now().difference(launchDate).inDays ~/ 365;
      print('Launched: $launchYear ($years years ago)');
    } else {
      print('Unlaunched');
    }
  }
}

var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = Spacecraft.unlaunched('Voyager III');
voyager3.describe();
```

# 변수
- var 로 명시적으로 타입을 지정하지 않고 변수 선언 가능
> var a = 'Hello';
- java하고 비슷하게 Object a = b; 형태로 사용
- kotlin하고 비슷하게 Object? 형태로 nullable하게 선언
> 다른점은 int 값도 객체기 때문에 int? 가능. kotlin은 가능은 하지만 primitive type은 Integer와 같은 랩퍼 클래스로 변경된

# late 변수
- 선언 이후에 초기화되는 non-nullable 변수를 선언하는 것
- 변수를 초기화를 지연하는 것
- late 변수의 초기화를 실패하였다면, 해당 변수를 사용 할 때 런타임 에러가 발생
- kotlin의 lateinit var 와 비슷한것 같다
```dart
late String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

# final 변수
- 할당 후 변경할 수 없는 변수
- 코틀린의 val과 비슷
- final 객체는 수정할 수 없지만 객체의 필드는 수정 가능
```dart
final name = 'Bob'; // 타입 어노테이션이 없음
final String nickname = 'Bobby';
name = 'Alice'; // 에러: final 변수는 한 번만 설정될 수 있습니다.
```

# const 변수
- 컴파일 타임 상수
- 클래스 레벨의 변수일 경우 static const 로 표기
- const 객체와 객체의 필드는 불변하기 때문에 변경 불가
```dart
const bar = 1000000; // 압력의 단위 (dynes/cm2)
const double atm = 1.01325 * bar; // 표준 대기

var foo = const [];
final bar = const [];
const baz = []; // `const []`와 동일
foo = [1, 2, 3]; // const [] 였음
baz = [42]; // 에러: 상수 변수는 값이 할당될 수 없습니다.

const Object i = 3; // i는 정수 값을 가지는 const Object입니다.
const list = [i as int]; // 타입 캐스트를 사용하세요.
const map = {if (i is int) i: 'int'}; // is와 컬렉션 if를 사용하세요.
const set = {if (list is List<int>) ...list}; // ...를 사용하여 전개.
```

# 연산자
|설명|연산자|결합법칙|
|---|---|---|
|unary postfix|expr++    expr--    ()    []    ?[]    .    ?.    !	|None|
unary prefix|	-expr    !expr    ~expr    ++expr    --expr      await expr   	|None|
multiplicative|	*    /    %  ~/	|Left|
additive|	+    -	|Left|
shift|	<<    >>    >>>	|Left|
bitwise AND|	&	|Left|
bitwise XOR|	^	|Left|
bitwise OR|		|Left|
relational and type test|	>=    >    <=    <    as    is    is!	|None|
equality|	==    !=   	|None|
logical AND|	&&	Left|
logical OR|		|Left|
if null|	??	|Left|
conditional|	expr1 ? expr2 : expr3	|Right|
cascade|	..    ?..	|Left|
assignment|	=    *=    /=   +=   -=   &=   ^=   etc.	|Right|

```
(employee as Person).firstName = 'Bob'; // employee가 null이거나 Person이 아니면 예외 발생

if (employee is Person) { // 예외 발생 안함
  // 타입 확인
  employee.firstName = 'Bob';
}

// a에 value를 할당합니다
a = value;
// b가 null이라면 value를 할당하고, 그렇지 않으면 그대로 둡니다.
b ??= value;

// name == null 이면 Guest 아니면 name 반환하는 함수
String playerName(String? name) => name ?? 'Guest';

// Cascade 노테이션
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;

// null check + Cascade 노테이션
querySelector('#confirm') // 객체 찾기.
  ?..text = 'Confirm' // 객체의 멤버 사용.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'))
  ..scrollIntoView();

// null check + Cascade 노테이션을 일반 구문으로 표시한 것
var button = querySelector('#confirm');
button?.text = 'Confirm';
button?.classes.add('important');
button?.onClick.listen((e) => window.alert('Confirmed!'));
button?.scrollIntoView();
```

# 주석
- // 주석
- /* 주석 */
- /// 문서화 주석

# 메타데이터 (어노테이션)
- @이름
```dart
class Television {
  /// Use [turnOn] to turn the power on instead.
  @Deprecated('Use turnOn instead')
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
  // ···
}
```

- 사용자 정의 어노테이션 만들기
```dart
class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}

@Todo('Dash', 'Implement this function')
void doSomething() {
  print('Do something');
}
```

# 라이브러리 사용
- 라이브러리는 API를 제공할 뿐만 아니라, 관리(privacy)의 단위가 됩니다
- 언더스코어(_)로 시작하는 식별자들은 오직 그 라이브러리 안에서만 보입니다
```dart
import 'dart:html';
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;
// foo만 import.
// import 'package:lib1/lib1.dart' show foo;

// foo를 제외하고 모두 import.
// import 'package:lib2/lib2.dart' hide foo;

// lib1의 Element 사용.
Element element1 = Element();

// lib3의 Element 사용.
lib2.Element element2 = lib2.Element();
```

# Enum
```dart
// 기본
// enum PlanetType { terrestrial, gas, ice }

// 심화
/// 우리 태양계에 있는 행성들과 행성들의 프로퍼티에 대한 Enum
enum Planet {
  mercury(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  venus(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  // ···
  uranus(planetType: PlanetType.ice, moons: 27, hasRings: true),
  neptune(planetType: PlanetType.ice, moons: 14, hasRings: true);

  /// 상수를 생성하는 생성자
  const Planet(
      {required this.planetType, required this.moons, required this.hasRings});

  /// 모든 인스턴스 변수들은 final
  final PlanetType planetType;
  final int moons;
  final bool hasRings;

  /// 발전된 enum은 getter와 다른 메서드를 지원합니다.
  bool get isGiant =>
      planetType == PlanetType.gas || planetType == PlanetType.ice;
}

// 사용
final yourPlanet = Planet.earth;

if (!yourPlanet.isGiant) {
  print('Your planet is not a "giant planet".');
}
```

# 상속
- extends 키워드를 쓴다
```dart
class Orbiter extends Spacecraft {
  double altitude;

  Orbiter(super.name, DateTime super.launchDate, this.altitude);
}
```

# mixin
- Mixin은 여러 클래스 계층에서 코드를 재사용하는 방법
- 받을 클래스에서 with 키워드를 쓴다
```dart
mixin Piloted {
  int astronauts = 1;

  void describeCrew() {
    print('Number of astronauts: $astronauts');
  }
}

class PilotedCraft extends Spacecraft with Piloted {
  // ···
}
```

# 인터페이스
- 모든 클래스는 인터페이스가 될 수 있다
- implement 키워드 사용
```dart
class MockSpaceship implements Spacecraft {
  // ···
}
```

# 추상 클래스
```dart
abstract class Describable {
  void describe();

  void describeWithEmphasis() {
    print('=========');
    describe();
    print('=========');
  }
}
```

# Async
- async과 await를 사용해 콜백 지옥에서 벗어나고 코드의 가독성을 높힐 수 있다
```dart
const oneSecond = Duration(seconds: 1);
// ···
Future<void> printWithDelay(String message) async {
  await Future.delayed(oneSecond);
  print(message);
}

/*
위의 메서드와 같다
Future<void> printWithDelay(String message) {
  return Future.delayed(oneSecond).then((_) {
    print(message);
  });
}
*/
```

# throw try on catch finally
- throw
```dart
if (astronauts == 0) {
  throw StateError('No astronauts.');
}
```
- try on catch finally
```dart
Future<void> describeFlybyObjects(List<String> flybyObjects) async {
  try {
    for (final object in flybyObjects) {
      var description = await File('$object.txt').readAsString();
      print(description);
    }
  } on IOException catch (e) {
    print('Could not describe object: $e');
  } finally {
    flybyObjects.clear();
  }
}
```
