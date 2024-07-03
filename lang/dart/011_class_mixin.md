# Mixins
- Mixins은 다수의 클래스 계층에서 클래스의 코드를 재사용 할 수 있는 방법
- 한 개 이상의 mixin 이름과 함께 with 키워드를 적용하여 mixin을 사용
```dart
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

# Mixin 선언
- mixin 선언을 사용하여 mixin을 정의
- Mixins과 mixin 클래스에 extends문을 사용할 수 없고, 제너러티브 생성자를 가지면 안 됨
- on 키워드로 사용할 수 있는 부모 클래스를 제한 가능
```dart
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```
```dart
class Musician {
  // ...
}
mixin MusicalPerformer on Musician {
  // ...
}
class SingerDancer extends Musician with MusicalPerformer {
  // ...
}
```

# mixin class
- Mixin Class는 클래스와 믹스인 둘 다로 사용할 수 있는 클래스를 정의할 수 있다
- 믹스인 클래스는 extends나 with 절을 가질 수 없지만, 추상 메서드를 통해 특정 메서드를 구현하도록 강제할 수 있다
```dart
abstract mixin class Musician {
  void playInstrument(String instrumentName);

  void playPiano() {
    playInstrument('Piano');
  }

  void playFlute() {
    playInstrument('Flute');
  }
}

class Virtuoso with Musician {
  @override
  void playInstrument(String instrumentName) {
    print('Plays the $instrumentName beautifully');
  }
}
```