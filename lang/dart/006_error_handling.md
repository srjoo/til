# Exception
- Java와 달리 모든 예외가 확인되지 않은 예외
- 메서드는 자신이 어떤 예외를 발생시킬지 선언하지 않는다

# throw
- 오류를 throwing 할 수 있다(일반적으로 Exception 혹은 Error 객체를 구현한다)
```dart
throw FormatException('Expected at least 1 section');
```
- Exception 객체가 아니라도 throw 할 수 있다 (일반적으로 사용하지는 않는다)
```dart
throw 'Out of llamas!';
```

# try catch finally
```dart
  try {
    // 예외가 발생할 가능성이 있는 코드
    int result = 10 ~/ 0;  // 정수 나눗셈으로 0으로 나누면 예외 발생
    print(result);
  } catch (e) {
    // 예외가 발생했을 때 실행되는 코드
    print('Caught an exception: $e');
  } finally {
    // 예외 발생 여부와 상관없이 항상 실행되는 코드
    print('This is finally block.');
  }
```

# try on catch
- on 키워드를 통해 특정 오류를 별도로 처리 가능
```dart
  try {
    int result = 10 ~/ 0;
    print(result);
  } on IntegerDivisionByZeroException {
    print('Cannot divide by zero');
  } catch (e) {
    print('Caught an exception: $e');
  }
```

# rethrow
- 예외를 부분적으로 처리한 후 다시 발생시켜 호출자가 예외를 처리할 수 있도록 할 수 있다
```dart
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // 런타임 에러
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // 호출자가 예외를 확인 할 수 있도록 허락
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
```

# assert
- 개발 중에 특정 조건이 참인지 확인하는 데 사용
- 조건이 거짓이면 프로그램 실행을 중단시키고 AssertionError 예외를 발생
- 프로덕션 코드에서는 무시됨
```dart
void main() {
  var text = 'Hello, World!';
  var number = 42;
  var urlString = 'https://example.com';

  // 변수가 null이 아닌지 확인
  assert(text != null);

  // 값이 100보다 작은지 확인
  assert(number < 100);

  // URL이 'https'로 시작하는지 확인
  assert(urlString.startsWith('https'));

  // URL이 'https'로 시작하는지 확인하고, 실패 시 메시지를 출력
  assert(
    urlString.startsWith('https'),
    'URL ($urlString) should start with "https".'
  );
}
```