# 패턴
- 패턴의 맥락과 모양에 따라 값 비교 혹은 값을 구조 분해하거나 둘 다를 수행
```dart
  var obj = [1, 2];

  switch (obj) {
    // List 패턴 [a, b]가 obj에 매칭되면
    // obj가 두 개의 필드를 가진 리스트일 때 일치하고,
    // 그 필드가 상수 하위 패턴 'a'와 'b'에 일치
    case [a, b]:
      print('$a, $b'); // 1, 2 출력
      break;
    default:
      print('No match');
  }
```

# 구조 분해
- 개체와 패턴이 일치하면 패턴은 개체의 데이터에 액세스하여 부분적으로 추출(코틀린의 componentN 비슷한듯)
- 패턴을 이용한 콜렉션의 구조 분해
```dart
  var numList = [1, 2, 3];
  // 리스트 패턴 [a, b, c]를 사용하여 numList의 세 요소를 구조 분해합니다.
  var [a, b, c] = numList;
  // 새로운 변수에 할당된 값을 사용합니다.
  print(a + b + c); // 6 출력
```
- 패턴 매칭을 통한 콜렉션의 구조 분해
```dart
  var list = ['a', 2];
  
  switch (list) {
    // 리스트의 첫 번째 요소가 'a' 또는 'b'일 때 두 번째 요소를 변수 c에 할당합니다.
    case ['a' || 'b', var c]:
      print(c); // 2 출력
      break;
    default:
      print('No match');
  }
```

# 변수 할당
- 일치하는 객체를 구조 해제 후 새 변수 혹은 기존 변수에 값을 할당
```dart
var (a, [b, c]) = ('str', [1, 2]);
```
- 변수 할당 패턴을 이용해서 별도 변수 없이 변수의 값을 교환할 수 있다
```dart
  var (a, b) = ('left', 'right'); // a와 b를 'left'와 'right'로 초기화
  (b, a) = (a, b); // 값을 교환
  print('$a $b'); // "right left" 출력
```

# switch문과 표현식
- switch 문과 표현식은 패턴 매칭을 사용하여 객체를 검사하고, 일치하는 경우에 특정 동작을 수행
- 패턴이 일치하면 객체를 구조 분해하고, 그렇지 않으면 다음 case로 넘어간다
```dart
var obj = (1, 2);

  switch (obj) {
    // obj가 1과 같으면 매칭
    case 1:
      print('one');
      break;

    // obj의 값 범위를 검사
    case >= 1 && <= 5:
      print('in range');
      break;

    // obj가 두 필드를 가진 레코드인지 검사하고, 필드를 'a'와 'b'에 할당
    case (var a, var b):
      print('a = $a, b = $b');
      break;

    // 위의 어떤 case와도 일치하지 않으면 실행
    default:
      print('no match');
  }
```
- 논리적 표현식을 통해 여러 case가 동일한 동작을 하게 할 수 있다
```dart
  var color = 'red';
  var isPrimary = switch (color) {
    case 'red' || 'yellow' || 'blue':
      true;
    default:
      false;
  };
  print(isPrimary); // true 출력
```

# for, for in에서 사용
- 루프를 사용하여 컬렉션의 값을 반복하고 구조 분해할 수 있다

```dart
Map<String, int> hist = {
  'a': 23,
  'b': 100,
};

for (var MapEntry(key: key, value: count) in hist.entries) {
  print('$key occurred $count times');
}

for (var MapEntry(:key, value: count) in hist.entries) {
  print('$key occurred $count times');
}
/*
 hist.entries는 MapEntry 객체의 이터러블을 반환합니다. 각 MapEntry 객체는 key와 value를 가지고 있으며, 이를 구조 분해하여 key와 count 변수에 할당
*/
```

# 다중 리턴값 구조 분해
- 레코드 형식으로 리턴된 여러 리턴값을 쉽게 분해 가능하다
```dart
var info = userInfo(json);
var name = info.$1;
var age = info.$2;
// 위를 아래와 같이 간단하게 변경 가능
var (name, age) = userInfo(json);
```

# 클래스 구조 분해
- 클래스 인스턴스를 구조 분해하여 해당 인스턴스의 속성을 얻을 수 있다
```dart
final Foo myFoo = Foo(one: 'one', two: 2);
var Foo(:one, :two) = myFoo;
print('one $one, two $two');
```

# 알제브라 데이터 타입
- 알제브라 데이터 타입(Algebraic Data Types, ADTs)은 서로 관련된 여러 타입을 하나의 타입 체계로 묶어 다룰 때 유용
- ADT 스타일을 사용하면 여러 타입에 대해 특정 동작을 구현할 필요 없이, 하나의 함수에서 이를 관리할 수 있다
- ADT를 사용해야 하는 경우
> - 관련된 타입의 그룹이 있을 때
> - 각 타입에 대해 특정 동작이 필요할 때
> - 각 타입 정의에 동작을 분산시키는 대신, 하나의 함수에서 모든 변형을 그룹화하고 싶을 때
- ADT의 장점
> - 유지 보수성
> - 확장성
> - 가독성
- 예제 : 도형의 면적 계산
```dart
import 'dart:math' as math;

sealed class Shape {}

class Square implements Shape {
  final double length;
  Square(this.length);
}

class Circle implements Shape {
  final double radius;
  Circle(this.radius);
}

double calculateArea(Shape shape) => switch (shape) {
  Square(length: var l) => l * l,
  Circle(radius: var r) => math.pi * r * r,
};

void main() {
  var shapes = [
    Square(3),
    Circle(2),
  ];

  for (var shape in shapes) {
    print('The area of the shape is ${calculateArea(shape)}');
  }
}
```

# json 데이터 검증
- 패턴 매칭을 통해 JSON 데이터의 구조를 더 간단하고 선언적으로 검증할 수 있다
```dart
var json = {
  'user': ['Lily', 13]
};
var {'user': [name, age]} = json;

/*
if (json is Map<String, Object?> &&
    json.length == 1 &&
    json.containsKey('user')) {
  var user = json['user'];
  if (user is List<Object> &&
      user.length == 2 &&
      user[0] is String &&
      user[1] is int) {
    var name = user[0] as String;
    var age = user[1] as int;
    print('User $name is $age years old.');
  }
}
*/

// 간단!
if (json case {'user': [String name, int age]}) {
  print('User $name is $age years old.');
}
```

# cast
- 캐스트 패턴을 사용하면 구조 분해 과정에서 타입 캐스트를 삽입하여 값을 특정 타입으로 변환할 수 있다
- 값이 예상한 타입이 아닐 경우 예외를 던지게 하여, 구조 분해된 값의 타입을 강제할 수 있다
```dart
  (num, Object) record = (1, "s");

  // 캐스트 패턴을 사용하여 각 값을 타입 변환
  // 만약 값이 지정된 타입과 일치하지 않으면 예외가 발생
  var (i as int, s as String) = record;

  print('i: $i'); // i: 1 출력
  print('s: $s'); // s: s 출력
```

# 상수	
- 특정 값이 상수와 일치하는지 검사하는 패턴
```dart
switch (number) {
  // Matches if 1 == number.
  case 1: // ... 
}

// List or map pattern:
case [a, b]: // ...

// List or map literal:
case const [a, b]: // ...
```

# 식별자
- 선언 컨텍스트: 식별자 이름을 사용하여 새 변수를 선언합니다. var (a, b) = (1, 2);
- 할당 컨텍스트: 식별자 이름을 사용하여 기존 변수에 할당합니다. (a, b) = (3, 4);
- 일치하는 컨텍스트: 명명된 상수 패턴으로 처리됩니다(이름이 인 경우 제외 _)
- 모든 컨텍스트의 와일드카드 식별자: 모든 값과 일치하고 이를 삭제합니다. case [_, var y, _]: print('The middle element is $y');
```dart
const c = 1;
switch (2) {
  case c: print('match $c');
  default: print('no match'); // Prints "no match".
 }
```

# 리스트
- 리스트 패턴을 사용하여 리스트의 요소를 매칭하고 추출
```dart
switch (obj) {
  // Matches if obj is a list with two elements.
  case [a, b]: // ... 
}
```

# 나머지 + 리스트
- 리스트의 요소 수가 고정되지 않을 때는 나머지 요소를 처리하기 위해 나머지 요소 패턴(...)을 사용할 수 있다
```dart
var [a, b, ..., c, d] = [1, 2, 3, 4, 5, 6, 7];
// Prints "1 2 6 7".
print('$a $b $c $d'); 
```
```dart
var [a, b, ...rest, c, d] = [1, 2, 3, 4, 5, 6, 7];
// Prints "1 2 [3, 4, 5] 6 7".
print('$a $b $rest $c $d');
```

# and 연산
```dart
switch ((1, 2)) {
  // Error, both subpatterns attempt to bind 'b'.
  case (var a, var b) && (var b, var c): // ... 
}
```

# or 연산
```dart
var isPrimary = switch (color) {
  Color.red || Color.yellow || Color.blue => true,
  _ => false
};
```

# map
- 맵 패턴을 사용하면 맵의 특정 키와 값을 매칭하고 구조 분해할 수 있다
- 패턴에 포함되지 않은 키는 무시
```dart
  var map = {
    'name': 'Alice',
    'age': 30,
    'city': 'Wonderland'
  };

  switch (map) {
    case {'name': var name, 'age': var age}:
      print('Name: $name, Age: $age'); // Name: Alice, Age: 30 출력
      break;
    default:
      print('No match');
  }
```

# Null-assert
- null-assert 패턴은 객체가 null이 아닌 경우에 매칭을 수행하며, 만약 객체가 null이라면 예외를 던집니다
```dart
List<String?> row = // ...

switch (row) {
  case ['user', var name!]: // ...
    // 'name' is a non-nullable string here.
}

(int?, int?) position = // ...

var (x!, y!) = position;
```

# Null 검사
- null-check 패턴은 값이 null이 아닌 경우에만 매칭을 수행
- null인 경우에는 매칭 실패로 처리
- 이를 통해 값이 null인지 확인한 후에 안전하게 값을 사용
```dart
String? maybeString = // ... maybeString is nullable with the base type String.
switch (maybeString) {
  case var s?: ...
    // 's' has type non-nullable String here.
}
```

# Object
- 객체의 특정 타입을 확인하고 해당 객체의 프로퍼티를 구조 분해할 때 사용
- 특정 타입의 객체만 매칭
- 객체 패턴은 타입이 일치하지 않으면 매칭에 실패합니다.
```dart
switch (shape) {
  // Matches if shape is of type Rect, and then against the properties of Rect.
  case Rect(width: var w, height: var h): // ...
}
```
```dart
// Binds new variables x and y to the values of Point's x and y properties.
var Point(:x, :y) = Point(1, 2);
```

# 괄호로 묶은 표현식
- 괄호를 사용한 패턴(Parenthesized Patterns)은 수식에서 괄호를 사용하는 것과 유사하게 패턴의 우선순위를 제어
- 낮은 우선순위를 가지는 패턴을 더 높은 우선순위가 필요한 위치에 삽입
```dart
// Binds new variables x and y to the values of Point's x and y properties.
var Point(:x, :y) = Point(1, 2);
```

# Record
- 레코드 객체를 매칭하고 필드를 구조 분해하는 데 사용
- 패턴과 동일한 형태의 레코드 객체에만 매칭
- 필드 이름을 포함하여 패턴을 정의함으로써 특정 필드를 추출
- 패턴 매칭이 실패하면 매칭되지 않은 것으로 처리
```dart
var (myString: foo, myNumber: bar) = (myString: 'string', myNumber: 1);
```
```dart
// Record pattern with variable subpatterns:
var (untyped: untyped, typed: int typed) = // ...
var (:untyped, :int typed) = // ...

switch (obj) {
  case (untyped: var untyped, typed: int typed): // ...
  case (:var untyped, :int typed): // ...
}

// Record pattern wih null-check and null-assert subpatterns:
switch (obj) {
  case (checked: var checked?, asserted: var asserted!): // ...
  case (:var checked?, :var asserted!): // ...
}

// Record pattern wih cast subpattern:
var (field: field as int) = // ...
var (:field as int) = // ...
```

# 관계식
-  주어진 상수와 비교하여 일치 여부를 결정하는 데 사용
```dart
String asciiCharType(int char) {
  const space = 32;
  const zero = 48;
  const nine = 57;

  return switch (char) {
    < space => 'control',
    == space => 'space',
    > space && < zero => 'punctuation',
    >= zero && <= nine => 'digit'
    // Etc...
  }
}
```

# 변수
- 매칭되거나 구조 분해된 값을 새로운 변수에 바인딩하는 데 사용
```dart
switch ((1, 2)) {
  // 'var a' and 'var b' are variable patterns that bind to 1 and 2, respectively.
  case (var a, var b): // ...
  // 'a' and 'b' are in scope in the case body.
}
```
```dart
switch ((1, 2)) {
  // Does not match.
  case (int a, String b): // ...
}
```

# 와일드 카드
- 변수 패턴이나 식별자 패턴처럼 동작하지만, 어떤 변수에도 값을 바인딩하거나 할당하지 않습니다
- 와일드카드 패턴은 주로 값의 특정 위치나 타입을 확인하면서 그 값을 무시할 때 유용
```dart
var list = [1, 2, 3];
var [_, two, _] = list;
```
```dart
switch (record) {
  case (int _, String _):
    print('First field is int and second is String.');
}
```