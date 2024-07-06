# 코딩 스타일
- 좋은 코드에 필요한 건 좋은 스타일
- 일관된 명명, 순서 및 서식은 동일한 코드가 동일하게 보이도록 한다

# 식별자
- UpperCamelCase이름은 각 단어의 첫 글자를 대문자로 시작합니다. 첫 글자도 포함합니다.
> 클래스, Enum, Typedef,유형 매개변수, Extension
> 각 단어의 첫 글자를 대문자로 써야 하며(첫 단어 포함), 구분 기호를 사용하지 않아야 합니다
```dart
class SliderMenu { ... }

class HttpRequest { ... }

typedef Predicate<T> = bool Function(T value);

extension MyFancyList<T> on List<T> { ... }

extension SmartIterable<T> on Iterable<T> { ... }
```


- lowerCamelCase이름은 각 단어의 첫 글자를 대문자로 표기합니다. 단, 첫 글자는 항상 소문자입니다. 약어일지라도 마찬가지입니다.
> 클래스 멤버, 최상위 정의, 변수, 매개변수, 명명된 매개변수, 상수
> 첫 단어를 제외한 각 단어의 첫 글자를 대문자로 쓰고, 구분 기호를 사용하지 않아야 합니다
```dart
var count = 3;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}

const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = Random();
}
```

- lowercase_with_underscores이름은 약어일지라도 소문자만 사용하고, 각 단어는 _. (마침표)로 구분합니다.
> 패키지, 디렉토리 및 소스 파일의 이름, import 가져올 때 이름 지정시
> 일부 파일 시스템은 대소문자를 구분하지 않으므로 많은 프로젝트에서 파일 이름을 모두 소문자로 지정해야 합니다. 구분 문자를 사용하면 이름을 해당 형식으로 읽을 수 있습니다. 구분 기호로 밑줄을 사용
```
my_package
└─ lib
   └─ file_system.dart
   └─ slider_menu.dart
   
import 'dart:math' as math;
import 'package:angular_components/angular_components.dart' as angular_components;
import 'package:js/js.dart' as js;
```

# 두 글자 이상인 단어나 약어는 대문자로 표기
- 대문자로 시작하는 약어는 읽기 어려울 수 있으며, 인접한 여러 약어는 모호한 이름으로 이어질 수 있습니다
```dart
// good
class HttpConnection {}
class DBIOPort {}
class TVVcr {}
class MrRogers {}

var httpRequest = ...
var uiHandler = ...
var userId = ...
Id id;

// bad
/*
class HTTPConnection {}
class DbIoPort {}
class TvVcr {}
class MRRogers {}

var hTTPRequest = ...
var uIHandler = ...
var userID = ...
ID iD;
*/
```

# 사용되지 않는 콜백 인자는 _ , __ 를 쓴다
- 콜백 구현에서 매개변수를 사용 하지 않는 경우 사용되지 않는 매개변수의 이름을 _ 로 표기하는게 관용적. 여러개인 경우 _ 의 개수를 늘린다
```dart
futureOfVoid.then((_) {
  print('Operation complete.');
});
```

# 비공개가 아닌 식별자는 _ 를 쓰지 않는다
- 다트에서는 밑줄을 사용하여 멤버와 최상위 선언을 비공개로 표시
- 따라서 비공개인 식별자만 _ 를 쓴다

# 헝가리 표기법과 같은 변수 앞에 접두사를 쓰지 말라
- Dart는 선언의 유형, 범위, 가변성 및 기타 속성을 알려줄 수 있으므로 식별자 이름에 이러한 속성을 인코딩할 이유가 없다
```dart
// good
defaultTimeout

// bad
kDefaultTimeout
```

# import 표시순
- 기능 import를 표시 후 라이브러리 경로 import
```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

# export는 마지막에 라인을 띄워서 별도로 표시
```dart
import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
```

# import를 알파벳순으로 정렬
```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'foo.dart';
import 'foo/foo.dart';
```

# 80 글자 이상을 줄을 피하라
- 가독성이 저하된다

# 모든 흐름 제어문에 중괄호 사용
- 단 한줄로 완결되는 경우 예외적으로 사용안해도 됨
```dart
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}

if (arg == null) return defaultValue;

if (overflowChars != other.overflowChars) {
  return overflowChars < other.overflowChars;
}
```