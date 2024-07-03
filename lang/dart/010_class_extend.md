# 상속
- 자식 클래스를 만들고 싶다면, extends를 사용
- 부모 클래스는 super로 참조 가능
```dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
```

# 재정의
- 자식 클래스는 연산자를 포함한 인스턴스 메서드, getter, setter를 오버라이드 가능
- 리턴 타입은 반드시 재정의되는 함수의 반환 타입(서브 타입도 가능)과 동일해야함
- 인자의 타입은 오버라이딩 되는 함수의 인수 타입(supertype도 가능)과 동일해야함
- 제네릭 메서드는 제네릭이 아닌 메서드를 재정의 할 수 없고, 그 반대도 동일
```dart
class Television {
  // ···
  set contrast(int value) {...}
}

class SmartTelevision extends Television {
  @override
  set contrast(num value) {...}
  // ···
}
```

# noSuchMethod()
- 코드가 존재하지 않는 함수나 인스턴스 변수에 접근하는 것을 감지, 처리할 수 있다