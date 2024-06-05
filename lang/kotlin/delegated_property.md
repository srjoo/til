# Delegated Property(위임 프로퍼티)
- "위임"은 자신이 직접 작업을 수행하지 않고 다른 객체에게 그 작업을 처리하도록 맡기는 디자인 패턴
- 프로퍼티 필드에 접근하는 getter/setter 메소드를 가지는 다른 객체를 만들어서 그 객체에 프로퍼티 필드 접근 로직을 위임

# 왜 위임을 써야 하나?
- 상속의 문제점 회피를 위해 쓴다
- 상위 클래스 내용이 변경되면 하위 클래스를 변경해야됨
- 불필요한 상위 클래스 메소드 구현
- 복잡한 구조로 기능 파악 어려움

# 상속과의 차이
- 상속은 상위 클래스의 모든 것을 받기 때문에 재구현할 필요가 없어 편리
- 상속은 높은 의존도 문제가 발생된다
- 일반적으로 상속은 하나의 클래스에만 가능
- 하지만 위임의 여러개가 가능

# 문법
- by를 통해 위임한다
- property의 게터/세터를 Delegate 객체에게 위임
```kotlin
class Example {
   var property: Type by Delegate()
}
```

- 프로퍼티 위임 객체인 Delegate 클래스는  getValue와 setValue 메소드를 제공해야 하며, Delegate 클래스를 단순화하면 아래와 같습니다.
```
class Delegate {
   operator fun getValue(...) { }
   operator fun setValue(..., value: Type) { }
}
```
- 실제 클래스 예제
```kotlin
class ObservableProperty(
    var propValue: Int, val changeSupport: PropertyChangeSupport
) {
    operator fun getValue(p: Person, prop: KProperty<*>): Int = propValue
    operator fun setValue(p: Person, prop: KProperty<*>, newValue: Int) {
        val oldValue = propValue
        propValue = newValue
        changeSupport.firePropertyChange(prop.name, oldValue, newValue)
    }
}
```