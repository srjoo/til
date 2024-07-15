# 상속
- 모든 kotlin 클래스는 Any 클래스를 상속 받는다
- Any는 equals(), hashCode(), toString()을 기본으로 가지고 있다
- open으로 표시하지 않는이상 기본적으로 모든 클래스는 final이다(상속 불가)
```kotlin
open class Base // Class is open for inheritance
```
- 상속 받은 클래스는 부모의 생성자를 반드시 호출해야 된다
```kotlin
class MyView : View {
    constructor(ctx: Context) : super(ctx)

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

# 메서드 재정의
- 상속 받은 클래스가 재정의를 위해서는 부모 클래스에서 open 으로 메서드를 선언해야 가능
```kotlin
open class Shape {
    open fun draw() { /*...*/ }
    fun fill() { /*...*/ }
}

class Circle() : Shape() {
    override fun draw() { /*...*/ }
}
```
- 재정의된 메서드를 재정의 금지하려면 final으로 선언
```kotlin
open class Rectangle() : Shape() {
    final override fun draw() { /*...*/ }
}
```
- super 키워드로 부모 클래스 중 원하는 클래스의 함수를 호출 할 수 있다
```kotlin
open class Rectangle {
    open fun draw() { /* ... */ }
}

interface Polygon {
    fun draw() { /* ... */ } // interface members are 'open' by default
}

class Square() : Rectangle(), Polygon {
    // The compiler requires draw() to be overridden:
    override fun draw() {
        super<Rectangle>.draw() // call to Rectangle.draw()
        super<Polygon>.draw() // call to Polygon.draw()
    }
}
```

# 속성 재정의
- 메서드 재정의와 동일하게 속성 역시 재정의 할 수 있다
```kotlin
open class Shape {
    open val vertexCount: Int = 0
}

class Rectangle : Shape() {
    override val vertexCount = 4
}
```
- val 속성을 var 속성으로 재정의할 수 있다. 반대는 불가능.
- val 속성이 본질적으로 get 메서드를 선언하며, 이를 var로 재정의하는 것은 파생 클래스에서 추가로 set 메서드를 선언하는 것이기 때문에 가능
```kotlin
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape // Always has 4 vertices

class Polygon : Shape {
    override var vertexCount: Int = 0  // Can be set to any number later
}
```