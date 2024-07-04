# abstract
- abstract 클래스는 인스턴스화할 수 없는 클래스
- 다른 클래스들이 상속받아 사용할 메서드와 속성을 정의하는 데 사용
```dart
abstract class Animal {
  void makeSound(); // 추상 메서드
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('Bark');
  }
}
```

# base
- base 클래스는 외부에서 상속받을 수 없고, 동일한 라이브러리 내에서만 상속 가능
- base 클래스를 상속 받은 클래스는 'base', 'final', 'sealed' 클래스여야만 한다(외부에서 상속 받는 것을 방지)
```dart
base class Vehicle {
  void drive() {
    print('Driving');
  }
}

// 같은 라이브러리 내에서만 상속 가능
base class Car extends Vehicle {
  @override
  void drive() {
    print('Driving a car');
  }
}
```

# final
- final 클래스는 상속할 수 없는 클래스
```dart
final class Animal {
  void makeSound() {
    print('Some sound');
  }
}

// class Dog extends Animal {} // 오류: 'final' 클래스는 상속할 수 없습니다.
```

# interface
- interface 클래스는 구현할 메서드와 속성을 정의할 수 있으며, 다른 클래스는 이 인터페이스를 구현해야함
```dart
class Animal {
  void makeSound() {}
}

interface class Flying {
  void fly();
}

class Bird extends Animal implements Flying {
  @override
  void makeSound() {
    print('Chirp');
  }

  @override
  void fly() {
    print('Flying');
  }
}
```

# sealed
- sealed 클래스는 현재 라이브러리 내에서만 서브클래스가 정의될 수 있으며, 새로운 서브클래스를 추가하려면 이 라이브러리에서만 가능
```dart
sealed class Shape {}

class Circle extends Shape {}
class Square extends Shape {}

// 다른 라이브러리에서는 Shape의 서브클래스를 정의할 수 없습니다.
```

# mixin
- mixin 클래스는 재사용 가능한 기능을 다른 클래스에 추가할 때 사용
```dart
mixin class Musical {
  void playPiano() {
    print('Playing piano');
  }
}

class Musician with Musical {}

void main() {
  var musician = Musician();
  musician.playPiano(); // 출력: Playing piano
}
```

