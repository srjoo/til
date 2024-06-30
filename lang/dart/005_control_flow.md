# 반복(Loops)

# for : 일반적인 스타일
```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```
- for 루프 내의 클로저는 인덱스 값을 캡처하기 때문에 javascript와 같이 index 값을 참조했을 때 문제가 없다
- 즉 예상 가능한대로 행동한다
```dart
  var callbacks = [];
  for (var i = 0; i < 2; i++) {
    callbacks.add(() => print(i));
  }

  for (final c in callbacks) {
    c(); // 0 출력, 1 출력
  }
```
# for-in : 현재 반복 횟수를 알 필요가 없는 경우 간결하고 가독성이 좋다
```dart
  var candidates = ['Alice', 'Bob', 'Charlie'];
  for (final candidate in candidates) {
    print('$candidate is being interviewed.');
  }
  // Alice is being interviewed.
  // Bob is being interviewed.
  // Charlie is being interviewed.
```
# 패턴 매칭을 사용하는 for-in : 패턴 매칭을 사용하여 이터러블의 값을 처리
```dart
class Candidate {
  final String name;
  final int yearsExperience;

  Candidate(this.name, this.yearsExperience);
}

void main() {
  var candidates = [
    Candidate('Alice', 5),
    Candidate('Bob', 3),
    Candidate('Charlie', 7)
  ];

  // name과 yearsExperience를 추출하여 사용한다
  for (final Candidate(:name, :yearsExperience) in candidates) {
    print('$name has $yearsExperience years of experience.');
  }
  // Alice has 5 years of experience.
  // Bob has 3 years of experience.
  // Charlie has 7 years of experience.
}
```
# forEach : 이터러블 클래스는 forEach 메서드를 제공
```dart
  var collection = [1, 2, 3];
  collection.forEach(print); // 1, 2, 3 출력
```
# while : 일반적인 스타일, 0번 이상 실행됨
```dart
while (!isDone()) {
  doSomething();
}
```
# do while : 일반적인 스타일, 1번 이상 실행됨
```dart
do {
  printLine();
} while (!atEndOfPage());
```
# break, continue : 일반적인 스타일
```dart
while (true) {
  if (shutDownRequested()) break; // while문 밖으로
  processIncomingRequests();
}

for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue; // candidate.interview() 생략
  }
  candidate.interview();
}
```

# 분기처리(Branches)

# if else : 일반적인 스타일
```dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```

# if case : if + 패턴 매칭, 값이 패턴과 일치할 때 해당 분기를 실행
```dart
  var pair = [3, 4];

  if (pair case [int x, int y]) {
    print('Coordinates: $x, $y'); // Coordinates: 3, 4 출력
  } else {
    throw FormatException('Invalid coordinates.');
  }
```
```dart
  var value = {'type': 'point', 'x': 5, 'y': 10};

  if (value case {'type': 'point', 'x': var x, 'y': var y}) {
    print('Point coordinates: ($x, $y)'); // Point coordinates: (5, 10) 출력
  } else {
    print('Not a point.');
  }
```

# if case Guard clause
- when 키워드를 사용하여 guard 절을 정의
- 케이스 본문이 실행될 추가 조건을 설정 되는 것
- guard 절이 거짓으로 평가되면 다음 if문으로 이동
```dart
  var pair = (5, 5);

  if (pair case (int a, int b) when a > b) {
    print('First element greater');
  } else if (pair case (int a, int b) when a == b) {
    print('Elements are equal'); // 출력됨
  } else {
    print('First element not greater');
  }
```

# switch : 다른 언어와 다르게 반드시 break 구문이 필요하지 않다. case 실행 후 자동으로 넘어감. 와일드카드 _는 그외 모든 케이스를 뜻한다
- 와일드카드는 default와 다르게 가장 마지막에 나오지 않아도 된다, 패턴 매칭의 모든 곳에서 사용된다(default는 switch문만 사용)
```dart
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
  case 'PENDING':
    executePending();
  case 'APPROVED':
    executeApproved();
  case 'DENIED':
    executeDenied();
  case 'OPEN':
    executeOpen();
  // case _: == default
  default:
    executeUnknown();
}
```
- switch에서 continue 사용하여 특정 label로의 goto의 효과를 낼 수 있다
```dart
switch (command) {
  case 'OPEN':
    executeOpen();
    continue newCase;     // Continues executing at the newCase label.
  
  case 'DENIED':          // Empty case falls through.
  case 'CLOSED':
    executeClosed();      // Runs for both DENIED and CLOSED,
  
  newCase:
  case 'PENDING':
    executeNowClosed();   // Runs for both OPEN and PENDING.
}
```

# switch 표현식 : switch 문을 사용하여 특정값을 할당 할 수 있다
- case 키워드로 시작하지 않는다
- 각 case는 단일 표현식
- case 패턴과 본문은 =>로 구분
- case는 쉼표로 구분, 선택적으로 마지막에 쉼표를 추가 가능
- 기본 케이스는 와일드 카드로만 사용(default 키워드 사용 불가)
```dart
  var grade = 'A';

  var result = switch (grade) {
    'A' => 'Excellent',
    'B' => 'Good',
    'C' => 'Fair',
    'D' => 'Poor',
    _ => 'Invalid grade',
  };

  print(result); // Excellent
```

# switch 포괄성 검사(Exhaustiveness Checking)
- switch 문이나 switch 표현식에서 모든 가능한 값이 처리되지 않을 경우 컴파일 타임 에러를 보고하는 기능
- bool? 타입의 변수를 처리하는 switch 문이 있을 때 true, false만 처리하면 null 처리 부분에 대한 누락을 알려주고 컴파일 오류가 발생
```dart
  bool? b = false;

  // Non-exhaustive switch on bool?, missing case to match null possiblity:
  switch (b) {
    case true:
      print('yes');
      break;
    case false:
      print('no');
      break;
  }
```
> default나 와일드카드로 그외 케이스를 처리해야한다
- 이러한 특성은 열거형(Enum)이나 밀봉 클래스(Sealed class)에서 유용하다. 이 두가지는 가능한 케이스가 모두 알려져 있어 포괄성이 보장됨
- Enum
```dart
enum TrafficLight { red, yellow, green }

void main() {
  var light = TrafficLight.red;

  switch (light) {
    case TrafficLight.red:
      print('Stop');
      break;
    case TrafficLight.yellow:
      print('Caution');
      break;
    case TrafficLight.green:
      print('Go');
      break;
  }
}
```
- Sealed Class : 새 서브타입이 추가되면 컴파일러가 누락된 케이스를 알려준다
```dart
sealed class Shape {
  double calculateArea();
}

class Square extends Shape {
  final double length;
  Square(this.length);

  @override
  double calculateArea() => length * length;
}

class Circle extends Shape {
  final double radius;
  Circle(this.radius);

  @override
  double calculateArea() => math.pi * radius * radius;
}

double calculateArea(Shape shape) => switch (shape) {
  Square(length: var l) => l * l,
  Circle(radius: var r) => math.pi * r * r
};

void main() {
  Shape shape = Square(4);
  print('Area: ${calculateArea(shape)}');
}
```

# switch Guard clause
- when 키워드를 사용하여 guard 절을 정의
- 케이스 본문이 실행될 추가 조건을 설정 되는 것
- guard 절이 거짓으로 평가되면 다음 케이스로 이동
```dart
  var pair = (3, 2);

  switch (pair) {
    case (int a, int b) when a > b:
      print('First element greater'); // 출력됨
      break;
    case (int a, int b):
      print('First element not greater');
      break;
  }
```