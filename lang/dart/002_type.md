# Dart Type System
- Dart는 타입이 안전한 프로그래밍 언어
- 변수값이 항상 변수의 정적 또는 안전한 타입과 일치하는지 확인하기 위해 정적 타입 검사와 런타임 검사를 사용
- 타입 안정성의 장점
>> 타입 관련 버그를 컴파일 타임에 찾을 수 있습니다.
>> 코드의 가독성이 높아집니다.
>> 코드의 유지관리가 쉬워집니다.
>> 더 나은 AOT(Ahead-Of-Time) 컴파일을 제공합니다.
- 상속시 메서드를 재정의 할 때 타입이 안전한 반환 값을 사용
```dart
class Animal {
  void chase(Animal a) { ... }
  Animal get parent => ...
}

class HoneyBadger extends Animal {
  @override
  void chase(Animal a) { ... }

//부모의 getter 메서드는 Animal을 반환합니다. 자식 클래스인 HoneyBadger의 getter 메서드의 반환 타입을 HoneyBadger 또는 Animal의 다른 서브타입으로 대체할 수 있지만, 관련이 없는 타입으로는 불가능
  @override
  HoneyBadger get parent => ... // OK
  // Root get parent => ... // error
}
```
- 상속시 메서드를 재정의할 때 타입이 안전한 매개변수를 사용
``` dart
class Animal {
  void chase(Animal a) { ... }
  Animal get parent => ...
}

class HoneyBadger extends Animal {
  @override
  void chase(Object a) { ... } // OK

  @override
  Animal get parent => ...
}

class Mouse extends Animal {...}

class Cat extends Animal {
// 자식 클래스 메서드의 매개변수는 부모 클래스 메서드의 매개변수 또는 해당 매개변수의 부모 타입과 동일한 타입이어야 합니다. 원래의 매개변수 타입을 대체하기 위해 서브타입을 사용하지 마세요. 그러면 매개변수 타입이 “좁아”집니다.
  @override
  void chase(Mouse x) { ... } // error
}
```
- dynamic 리스트를 타입이 지정된 리스트처럼 사용하면 안된다
```dart
class Cat extends Animal { ... }

class Dog extends Animal { ... }

void main() {
  List<Cat> foo = <dynamic>[Dog()]; // Error
  List<dynamic> bar = <dynamic>[Dog(), Cat()]; // OK
}
```
- 런타임 검사는 컴파일 타임에 감지하지 못하는 이슈를 처리한다.(Exception 발생)
```dart
void main() {
  List<Animal> animals = [Dog()];
  List<Cat> cats = animals as List<Cat>;
}
```
- 타입 추론 : Analyzer는 필드, 메서드, 지역 변수와 대부분의 제네릭 타입 인자를 추론합니다. Analyzer가 특정 타입을 추론할 만큼 충분한 정보가 없다면, dynamic 타입을 사용
```dart
Map<String, dynamic> arguments = {'argA': 'hello', 'argB': 42}; // 명시적 지정
var arguments = {'argA': 'hello', 'argB': 42}; // Map<String, Object> 추론
// Map 리터럴은 자신의 엔트리를 참고하여 타입을 추론하고 변수는 map 리터럴의 타입을 참고하여 자신의 타입을 추론합니다. 이 map에서 키는 모두 문자열이지만 값은 다른 타입 (같은 상한(upper bound) 타입인 Object를 갖는 String과 int)입니다. 따라서 Map 리터럴의 타입은 arguments 변수의 타입인 Map<String, Object>입니다.
```

# Numbers (int, double)
- **int** (var i = 1)
> 사용하는 플랫폼에 따라서 정수 값은 64비트 이하로 표현됩니다. 네이티브 플랫폼에서는 -263 ~ 263 - 1 까지 표현됩니다. 웹에서는, Javascript numbers (가수부가 없는 64-bits 부동소수점 표현) -253 to 253 - 1 사이의 수로 표현됩니다.
- **double** (var d = 1.1)
> IEEE 754 standard를 따라 64-bit (배정도) 부동 소수점 표현을 사용합니다.	
- **num** (num n = 1; n += 1.1)
> int와 double을 모두 쓸 수 있다

# Strings (String)
```dart
var s = 'abcd';
var s2 = "abcd";
var s3 = "$s, $(s.toUpperCase())";
var s4 = 'String '
    'concatenation'
    " works even over line breaks.";
var s5 = '''
You can create
multi-line strings like this one.
''';
var s6 = r'In a raw string, not even \n gets special treatment.'; // raw string
```

# Booleans (bool)
- true or false

# Records ((value1, value2))
- 괄호로 묶인 명명된 필드 또는 위치 필드의 쉼표로 구분된 목록
```dart
var record = ('first', a: 2, b: true, 'last');
```
```dart
(int, int) swap((int, int) record) {
  var (a, b) = record;
  return (b, a);
}
```
```dart
// Record type annotation in a variable declaration:
(String, int) record;

// Initialize it with a record expression:
record = ('A string', 123);
```
- 레코드 표현식에서 이름은 각 필드 값 앞에 콜론이 붙습니다
```dart
// Record type annotation in a variable declaration:
({int a, bool b}) record;

// Initialize it with a record expression:
record = (a: 123, b: true);

({int a, int b}) recordAB = (a: 1, b: 2);
({int x, int y}) recordXY = (x: 3, y: 4);

// Compile error! These records don't have the same type.
// recordAB = recordXY;
```
- 위치 필드의 이름을 지정할 수도 있지만 이러한 이름은 순전히 문서화용이며 레코드 유형에 영향을 주지 않습니다.
```dart
(int a, int b) recordAB = (1, 2);
(int x, int y) recordXY = (3, 4);

recordAB = recordXY; // OK.
```
- 레코드의 필드는 getter를 통해 액세스 가능, 이름이 있는 경우 이름으로 없는 경우 순서 순으로 $n
```dart
var record = ('first', a: 2, b: true, 'last');

print(record.$1); // Prints 'first'
print(record.a); // Prints 2
print(record.b); // Prints true
print(record.$2); // Prints 'last'
```
- 두 레코드의 모양 (필드 집합) 이 동일 하고 해당 필드의 값이 동일한 경우 두 레코드는 동일하다
```dart
(int x, int y, int z) point = (1, 2, 3);
(int r, int g, int b) color = (1, 2, 3);

print(point == color); // Prints 'true'.
```
- 레코드를 사용하여 리턴시 여러 값을 동시에 리턴 가능하다
```dart
// Returns multiple values in a record:
(String, int) userInfo(Map<String, dynamic> json) {
  return (json['name'] as String, json['age'] as int);
}

final json = <String, dynamic>{
  'name': 'Dash',
  'age': 10,
  'color': 'blue',
};

// Destructures using a record pattern:
var (name, age) = userInfo(json);

/* Equivalent to:
  var info = userInfo(json);
  var name = info.$1;
  var age  = info.$2;
*/
```

# Lists (List, arrays로도 부릅니다.)
- 별다른 문자 없이 쉽게 아래와 같이 사용 가능
```dart
var list = [1, 2, 3]; // Dart는 list를 List<int> 타입으로 추정
```
- trailing comma 사용 가능(뒤에 추가적인 요소가 없더라도 , 사용해도 문법 오류 발생 안함), 값 붙여넣을 때 오류를 방지한다
```dart
var list = [
1,
2,
3,
];
```
- 상수로 선언 가능하다
```
var constantList = const [1, 2, 3];
// constantList[1] = 1; // 이 라인은 에러를 발생시킵니다.
```
- 전개 연산자로 표현할 수 있다
```dart
var list = [1, 2, 3];
var list2 = [0, ...list]; // list2 = [0, 1, 2, 3];
// var list2 = [0, ...?list]; // list가 null일 경우 exception을 피할 수 있다
```

# Sets (Set)
- set은 유니크한 항목들로 이루어진 정렬되지 않은 컬렉션
```dart
var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'}; // Dart는 halogens을 Set<String> 타입으로 추정

var names = <String>{};
// Set<String> names = {}; // 이 코드도 작동합니다.
// var names = {}; // Set이 아닌 map을 생성합니다.
```
- 상수로 선언 가능하다
```dart
final constantSet = const {
  'fluorine',
  'chlorine',
  'bromine',
  'iodine',
  'astatine',
};
// constantSet.add('helium'); // 이 라인은 에러를 발생시킵니다.
```

# Maps (Map)
- map은 키와 값으로 구성된 객체
- 키와 값 모두 어떤 타입의 객체든 할당이 가능
- 각 키들은 유일, 값은 중복 가능
```dart
var gifts = {
  // 키:    값
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
}; // gifts의 타입을 Map<String, String>로 추정

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
}; // nobleGases의 타입을 Map<int, String>로 추정
```
- Map 생성자로 생성 가능
```dart
var gifts = Map<String, String>();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = Map<int, String>();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```
- 존재하지 않는 키에 접근하면 null이 반환된다
- 상수로 선언 가능하다
```dart
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // 이 라인은 에러를 발생시킵니다.
```

# List, Set, Map의 제어 흐름 연산자
- 컬렉션 if를 사용 가능
```dart
var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];
```
- 컬렉션 if-case를 사용 가능
```dart
var nav = ['Home', 'Furniture', 'Plants', if (login case 'Manager') 'Inventory'];
```
- 컬렉션 for를 사용 가능
```dart
var listOfInts = [1, 2, 3];
var listOfStrings = ['#0', for (var i in listOfInts) '#$i']; // listOfStrings = ['#0', '#1', '#2', '#3'];
```

# Runes (Runes; 때때로 characters API로 대체됩니다.)
- 문자열의 유니코드 코드 포인트

# Symbols (Symbol)
- Dart 프로그램에 선언된 연산자나 식별자
- Symbol은 거의 필요하지 않지만 코드 압축(minification) 후 식별자의 이름이 변경되더라도 symbol은 변경되지 않기 때문에 식별자를 통한 API 참조에 유용
- symbol를 가져오려면 symbol 리터럴을 사용하면 됩니다. Symbol 리터럴은 # 뒤에 식별자를 위치
```dart
#radix
#bar
```

# null 값 (Null)

# 특수 타입
- Object: Null을 제외한 모든 Dart 클래스의 부모 클래스.
- Enum: 모든 eunm의 부모 클래스.
- Future, Stream: 비동기 지원에서 사용됩니다.
- Iterable: for-in 루프 그리고 동기식 제너레이터 함수에서 사용됩니다.
- Never: 식(expression)의 평가를 완료할 수 없음을 나타냅니다. 항상 예외를 발생시키는 함수에서 보통 사용됩니다.
- dynamic: 정적 타입 체킹의 비활성화를 의미합니다. 대개 Object 또는 Object?를 대신 사용하세요.
- void: 값이 사용되지 않는다는 것을 의미합니다. 보통 반환 타입으로 사용됩니다.

# Generics
- 제네릭 타입을 적절하게 명시한 코드는 더 잘 작성된 코드
- 코드 중복를 줄이기 위해 제네릭을 사용
- 관례상 대부분의 타입 변수는 E, T, S, K, V 같은 single-letter
```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}

abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}

abstract class Cache<T> { // Generic 사용으로 위의 클래스 2 + 다른 형태가 나오더라도 대응 가능
  T getByKey(String key);
  void setByKey(String key, T value);
}
```
- 컬렉션 리터럴로 선언할 때 타입을 선언해주는 것이 좋다
```
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>{'Seth', 'Kathy', 'Lars'};
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};

var nameSet = Set<String>.from(names);
var views = Map<int, View>();
```
- Dart 제네릭 타입은 구체화되어 있다. Java는 아니다. 그것은 런타임에 타입들에 대한 정보를 가져온다는 것을 의미
```dart
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true

// Java의 제네릭은 erasure을 사용하고, 제네릭 타입 매개변수는 런타임에 삭제됩니다. 자바에서 어떤 객체가 List인지 테스트 할 수는 있지만, List<String>인지 테스트 하는 것은 불가능
```
- 매개변수화된 타입 제한
```dart
class Foo<T extends Object> {
  // Foo에게 제공되는 T 타입은 반드시 non-nullable 입니다.
}

class Foo<T extends SomeBaseClass> {
  // 클래스 구현 ...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}

var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();

var foo = Foo();
print(foo); // 'Foo<SomeBaseClass>'의 인스턴스

// var foo = Foo<Object>(); // error
```
- 메서드와 함수에도 사용 가능
```dart
T first<T>(List<T> ts) {
  // 초기 작업 또는 에러 확인, 그리고 ...
  T tmp = ts[0];
  // 추가적인 확인 또는 프로세싱 ...
  return tmp;
}
```

# typedef
- 타입 앨리어스는 타입을 참조하는 간편한 수단
```dart
typedef IntList = List<int>;
IntList il = [1, 2, 3];

typedef ListMapper<X> = Map<X, List<X>>;
Map<String, List<String>> m1 = {}; // 타입 선언이 장황합니다.
ListMapper<String> m2 = {}; // 위와 같지만 더 깔끔하고 짧습니다.

typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```