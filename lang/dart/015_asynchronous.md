# 비동기(Asynchronous) 함수
- Future 또는 Stream 객체를 반환하는 함수
- I/O 같이 시간이 오래 걸릴 수도 있는 작업이 완료되기를 기다리지 않고, 값을 반환할 수 있게 해준다

# Future와 Stream 타입
- Dart 언어와 라이브러리는 객체의 값을 미래에 얻을 수 있다는 것을 나타내기 위해 Future와 Stream을 사용
> 예를 들어, 결국에 int 값을 얻게 되는 약속(promise)은 Future<int> 타입입니다. int의 시리즈를 얻을 수 있는 약속은 Stream<int> 타입



# Future
- async와 await을 사용
```dart
Future<void> checkVersion() async {
  var version = await lookUpVersion();
  // version 변수를 사용...
}
```
- Future API 사용
- async 함수 안에 여러 개의 await를 사용해도 된다
```dart
var entrypoint = await findEntryPoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```
- await 표현식에서 표현식의 값은 보통 Future이고 Future가 아니라면 자동으로 Future가 값을 감싸게 된다
- await 표현식은 그 객체가 사용 가능해질 때까지 실행을 멈춘다
- 함수가 쓸모 있는 값을 반환하지 않는다면, 반환 타입을 Future<void>로 지정


# async 동작 순서
1. async 비동기 함수는 시간이 많이 걸리는 작업을 수행할 수 있지만 이러한 작업을 기다리지 않는다
2. sync 함수는 첫 번째 await 구문을 찾을 때까지 실행
3. Future object를 반환하고, await 구문이 끝난 뒤 코드 실행을 재개

# Stream
- async 와 비동기 for 루프 (await for)을 사용
```dart
await for (varOrType identifier in expression) {
  // Stream이 값을 내놓을 때마다 실행됩니다.
}
// 위 표현식의 값은 반드시 Stream 타입
```
- Stream에 대한 리스닝을 끝내고 싶다면, for 루프를 끝내고 stream을 unsubscribe하는 break나 return을 사용
```dart
void main() async {
  // ...
  await for (final request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```

# 동시성(concurrency)
- async-await, isolate 그리고 Future, Stream과 같은 클래스로 동시 프로그래밍을 지원

# Isolate
- 다트(Dart)에서 Isolate는 병렬 처리를 위한 기본 단위
- Isolate는 서로 독립적으로 실행되는 격리된 메모리 영역을 갖는 스레드와 유사한 개념
- 앱의 모든 Dart 코드는 isolate 안에서 실행
- 다트의 Isolate는 서로 다른 Isolate 간에 직접 메모리를 공유하지 않으며, 메시지 전달을 통해서만 통신
> 병렬 처리를 안전하고 효율적으로 수행
- Isolate에서 실행된 Dart 코드가 종료된 후에도 이벤트를 처리해야 하는 경우에는 isolate가 계속 유지
- 이벤트의 처리가 끝난 후, isolate는 종료

# Main isolate
- Main isolate에 대해 전혀 고려할 필요없다
- 프로그램의 실행이 시작되는 스레드
- main isolate의 이벤트 큐에는 리페인트 요청, 클릭된 알림 또는 기타 UI 이벤트가 포함
- main() 메서드가 실행된 후에 이벤트 큐의 처리가 시작되며, 이때 리페인트 이벤트가 첫 번째로 처리
- 동기 명령이 긴 처리 시간을 소요한다면, 앱의 반응성은 떨어진다

# 백그라운드 워커
- 긴 시간을 소요하는 계산 때문에 UI가 반응하지 않는다면, 해당 계산을 워커 isolate로 옮기는 것을 고려할 수 있다
- 워커 isolate는 종료될 때 계산 결과를 메시지로 반환
```dart
void main() async {
  // 데이터 읽기.
  final jsonData = await Isolate.run(_readAndParseJson);

  // 데이터 사용.
  print('Number of JSON keys: ${jsonData.length}');
}

Future<Map<String, dynamic>> _readAndParseJson() async {
  final fileData = await File(filename).readAsString();
  final jsonData = jsonDecode(fileData) as Map<String, dynamic>;
  return jsonData;
}
```
1. Isolate.run()은 백그라운드 워커인 isolate를 생성하고 main()은 결과를 기다립니다.

2. 생성된 isolate는 run()의 인자로 넘겨진 함수를 (위에서는 _readAndParseJson()) 실행합니다.

3. Isolate.run()은 return이 반환하는 결과를 main isolate에 전달하고 워커 isolate를 셧다운합니다.

4. 워커 isolate는 결과를 홀딩하고 있는 메모리를 main isolate에게 전달합니다. 데이터를 복사하지 않습니다. 워커 isolate는 해당 객체를 전달할 수 있는지 검증하는 작업을 수행합니다.

> Main isolate의 코드는 계속해서 실행되기 때문에 Isolate.run()의 결과는 항상 Future 입니다. Main isolate와 워커 isolate는 동시에 실행되기 때문에 워커 isolate가 실행하는 계산이 동기적이든 아니든 main isolate에 영향을 주지 않습니다.