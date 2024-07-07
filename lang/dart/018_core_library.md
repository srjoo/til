# dart:core
- 모든 Dart 프로그램에 내장된 유형, 컬렉션 및 기타 핵심 기능

# dart:async, package:async
- 비동기 프로그래밍을 지원하며, Future 및 Stream과 같은 클래스들을 포함
- package:async는 Future와 Stream 타입에 대한 추가 유틸리티 제공
```dart
import 'dart:async';

void main() {
  Future.delayed(Duration(seconds: 2), () {
    return 'Hello, Dart!';
  }).then((value) {
    print(value); // 출력: Hello, Dart! (2초 후)
  });
}
```

# dart:collection, package:collection
- dart:core의 컬렉션 지원을 보완하는 클래스 및 유틸리티 제공
- package:collection은 추가적인 컬렉션 구현 및 함수들을 제공
```dart
import 'dart:collection';

void main() {
  Queue<int> queue = Queue();
  queue.addAll([10, 20, 30]);
  print(queue); // 출력: {10, 20, 30}
}
```

# dart:convert, package:convert
- JSON 및 UTF-8과 같은 다양한 데이터 표현 간의 변환을 위한 인코더 및 디코더를 제공
- package:convert는 추가적인 인코더 및 디코더를 제공
```dart
import 'dart:convert';

void main() {
  String jsonString = '{"name": "John", "age": 30}';
  Map<String, dynamic> user = jsonDecode(jsonString);
  print(user['name']); // 출력: John

  String encoded = jsonEncode(user);
  print(encoded); // 출력: {"name":"John","age":30}
}
```

# dart:developer
- 디버거 및 인스펙터와 같은 개발자 도구와의 상호작용을 지원(Native JIT 및 dartdevc에서만 사용 가능)

# dart:math
- 수학 상수 및 함수, 랜덤 숫자 생성기를 제공
```dart
import 'dart:math';

void main() {
  print(pi); // 출력: 3.141592653589793

  Random random = Random();
  print(random.nextInt(100)); // 0부터 99 사이의 랜덤 정수 출력
}
```

# dart:typed_data, package:typed_data
- 고정 크기 데이터(예: unsigned 8-byte integers)와 SIMD 숫자 타입을 효율적으로 처리하는 리스트 제공
- package:typed_data는 추가적인 클래스 및 함수를 제공합니다.
```dart
import 'dart:typed_data';

void main() {
  Uint8List uint8list = Uint8List.fromList([0, 1, 2, 3]);
  print(uint8list); // 출력: [0, 1, 2, 3]
}
```

# dart:ffi, package:ffi
- 외부 함수 인터페이스(FFI)를 사용하여 다트 코드가 네이티브 C API를 사용할 수 있도록 합니다
- package:ffi는 다트 문자열과 C 문자열 간의 변환을 지원하는 유틸리티 포함
```dart
import 'dart:ffi';
import 'package:ffi/ffi.dart';

// C 함수 시그니처 정의
typedef CAddFunc = Int32 Function(Int32 a, Int32 b);
typedef DartAddFunc = int Function(int a, int b);

void main() {
  // 동적 라이브러리 로드
  final dylib = DynamicLibrary.open('path_to_library.so');

  // C 함수 로드
  final add = dylib.lookupFunction<CAddFunc, DartAddFunc>('add');

  // C 함수 호출
  final result = add(3, 4);
  print('3 + 4 = $result');
}
```

# dart:io, package:io
- 파일, 소켓, HTTP, 및 기타 I/O를 지원하여 웹이 아닌 애플리케이션에서 사용
- package:io는 ANSI 색상 지원, 파일 복사, 표준 종료 코드 등의 기능 제공
```dart
import 'dart:io';

void main() async {
  // 파일 쓰기
  final file = File('example.txt');
  await file.writeAsString('Hello, Dart!');

  // 파일 읽기
  final contents = await file.readAsString();
  print(contents); // 출력: Hello, Dart!
}
```

# dart:isolate
- 독립적인 작업자(worker)로서 스레드와 유사한 개념인 Isolate를 사용한 동시성 프로그래밍을 지원
```dart
import 'dart:isolate';

void main() async {
  final receivePort = ReceivePort();

  // 새로운 Isolate 생성
  await Isolate.spawn(isolateEntry, receivePort.sendPort);

  // Isolate로부터 메시지 수신
  receivePort.listen((message) {
    print('Received: $message');
    receivePort.close();
  });

  // 새로운 Isolate로 메시지 전송
  receivePort.sendPort.send('Hello from main isolate!');
}

void isolateEntry(SendPort sendPort) {
  final port = ReceivePort();

  // 메인 Isolate로 포트 전송
  sendPort.send(port.sendPort);

  // 메시지 수신 및 응답
  port.listen((message) {
    print('Received in isolate: $message');
    sendPort.send('Hello from new isolate!');
  });
}
```

# dart:mirrors
- 기본적인 반영(reflection)을 지원하며, 내성(introspection)과 동적 호출(dynamic invocation)을 제공
- 실험적 기능으로 네이티브 JIT에서만 지원(Flutter에서 지원안함)
```dart
import 'dart:mirrors';

class Person {
  final String name;
  final int age;

  Person(this.name, this.age);
}

void main() {
  final person = Person('Alice', 30);

  // Person 클래스의 메타데이터 가져오기
  final mirror = reflect(person);
  final classMirror = mirror.type;

  // 클래스 이름 출력
  print('Class: ${classMirror.simpleName}');

  // 필드 이름과 값 출력
  classMirror.declarations.forEach((symbol, declaration) {
    if (declaration is VariableMirror) {
      final fieldName = MirrorSystem.getName(symbol);
      final fieldValue = mirror.getField(symbol).reflectee;
      print('$fieldName: $fieldValue');
    }
  });
```

# dart:html
- 웹 기반 애플리케이션을 위한 HTML 요소 및 기타 리소스를 제공

# dart:indexed_db
- 인덱스를 지원하는 클라이언트 측 키-값 저장소를 제공

# dart:js, dart:js_util, package:js
- dart:js_util은 상호 운용성을 위한 저수준 프리미티브를 제공
- 일반적으로 상호 운용성을 더 간결하게 표현할 수 있는 package:js의 상위 수준 애너테이션을 사용하는 것이 좋다
- JavaScript 상호 운용성에 대한 자세한 내용은 관련 문서를 참조하십시오. dart:js의 직접 사용은 권장되지 않는다

# dart:svg
- 벡터 그래픽스를 제공

# dart:web_audio
- 브라우저에서 고품질 오디오 프로그래밍을 지원

# dart:web_gl
- 브라우저에서 3D 프로그래밍을 지원