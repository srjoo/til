# 패턴
- 패턴의 맥락과 모양에 따라 값 비교 혹은 값을 구조화하거나 둘 다를 수행
```dart
switch (obj) {
  // List pattern [a, b] matches obj first if obj is a list with two fields,
  // then if its fields match the constant subpatterns 'a' and 'b'.
  case [a, b]:
    print('$a, $b');
}
```

# 구조 분해
- 개체와 패턴이 일치하면 패턴은 개체의 데이터에 액세스하여 부분적으로 추출(코틀린의 componentN 비슷한듯)
```dart
var numList = [1, 2, 3];
// List pattern [a, b, c] destructures the three elements from numList...
var [a, b, c] = numList;
// ...and assigns them to new variables.
print(a + b + c);

switch (list) {
  case ['a' || 'b', var c]:
    print(c);
}
```

# 변수 할당
- 일치하는 객체를 구조 해제 후 새 변수를 바인딩하는 대신 기존 변수 에 값을 할당
```dart
// Declares new variables a, b, and c.
var (a, [b, c]) = ('str', [1, 2]);
```

# 표현식
```dart
switch (obj) {
  // Matches if 1 == obj.
  case 1:
    print('one');

  // Matches if the value of obj is between the constant values of 'first' and 'last'.
  case >= first && <= last:
    print('in range');

  // Matches if obj is a record with two fields, then assigns the fields to 'a' and 'b'.
  case (var a, var b):
    print('a = $a, b = $b');

  default:
}

var isPrimary = switch (color) {
  Color.red || Color.yellow || Color.blue => true,
  _ => false
};

switch (shape) {
  case Square(size: var s) || Circle(size: var s) when s > 0:
    print('Non-empty symmetric shape');
}
```

# for에서 사용
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
```

# 다중 리턴값 구조 분해
```dart
var info = userInfo(json);
var name = info.$1;
var age = info.$2;
// 위를 아래와 같이 간단하게 변경 가능
var (name, age) = userInfo(json);
```

# 클래스 구조 분해
```dart
final Foo myFoo = Foo(one: 'one', two: 2);
var Foo(:one, :two) = myFoo;
print('one $one, two $two');
```

# 대수 데이터 유형
```dart
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
      Circle(radius: var r) => math.pi * r * r
    };
```

# json 데이터 검증
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
```dart
(num, Object) record = (1, "s");
var (i as int, s as String) = record;
```

# 상수
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