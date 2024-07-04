# 확장 메서드
- 이미 존재하는 라이브러리에 기능을 추가
- extension <extension name>? on <type> {
  (<member definition>)*
}
- 확장의 이름은 생략할 수 있다
```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
  // ···
}

import 'string_apis.dart';
// ···
print('42'.parseInt()); // Extension 메서드 사용.
```
- 확장은 제네릭 타입 매개변수를 가질 수 있다
```dart
extension MyFancyList<T> on List<T> {
  int get doubleLength => length * 2;
  List<T> operator -() => reversed.toList();
  List<List<T>> split(int at) => [sublist(0, at), sublist(at)];
}
```
- dynamic 타입의 변수에 확장 메서드를 사용할 수 없다
- 확장 메서드는 정적으로 생성되기 때문에, static 함수를 호출하는 것만큼 빠르다
- api 충돌 발생시 show, hide로 설정하면 된다
```dart
// String 확장 메서드인 parseInt()를 정의하는 라이브러리.
import 'string_apis.dart';

// parseInt()를 정의하는 또다른 라이브러리.
// hide를 사용하여 NumberParsing2의 확장 메서드를 숨깁니다.
import 'string_apis_2.dart' hide NumberParsing2;

// ···
// 'string_apis.dart'에 정의된 parseInt()를 사용합니다.
print('42'.parseInt());
```
```dart
// 두 라이브러리 모두 parseInt()를 가지는
// String에 대한 확장을 가지고 있고, 해당 확장들은 다른 이름을 가지고 있습니다.
import 'string_apis.dart'; // Contains NumberParsing extension.
import 'string_apis_2.dart'; // Contains NumberParsing2 extension.

// ···
// print('42'.parseInt()); // 작동하지 않습니다.
print(NumberParsing('42').parseInt());
print(NumberParsing2('42').parseInt());
```
- 같은 이름을 가지고 있을 시 다른 이름으로 지정해서 사용한다
```dart
// 두 라이브러리 모두 parseInt() 확장 메서드를 가지는 NumberParsing 메서드를 가지고 있습니다.
// 'string_apis_3.dart'의 NumberParsingOne 확장도 parseNum()을 가지고 있습니다.
import 'string_apis.dart';
import 'string_apis_3.dart' as rad;

// ···
// print('42'.parseInt()); // Doesn't work.

// string_apis.dart의 ParseNumbers 확장을 사용합니다.
print(NumberParsing('42').parseInt());

// string_apis_3.dart의 ParseNumbers 확장을 사용합니다.
print(rad.NumberParsing('42').parseInt());

// string_apis_3.dart만 parseNum()을 가지고 있습니다.
print('42'.parseNum());
```