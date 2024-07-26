# Object expressions
- 익명 클래스의 객체를 생성한다
- object 키워드로 시작
```kotlin
val helloWorld = object {
    val hello = "Hello"
    val world = "World"
    // object expressions extend Any, so `override` is required on `toString()`
    override fun toString() = "$hello $world"
}

print(helloWorld)
```
- 상속 혹은 인터페이스 가능
```kotlin
open class A(x: Int) {
    public open val y: Int = x
}

interface B { /*...*/ }

val ab: A = object : A(1), B {
    override val y = 15
}
```
```kotlin
window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { /*...*/ }

    override fun mouseEntered(e: MouseEvent) { /*...*/ }
})
```
- 함수를 통해 리턴할 경우 상속 받은 객체가 없으면 Any, 상속받은 객체가 있으면 상속받은 객체가 리턴되는 것으로 취급된다, 단 private 함수거나 local 변수일 경우 새롭게 선언한 속성 접근 가능
```kotlin
class C {
    private fun getObject() = object {
        val x: String = "x"
    }

    fun printX() {
        println(getObject().x)
    }
}
```
```kotlin
interface A {
    fun funFromA() {}
}
interface B

class C {
    // The return type is Any; x is not accessible
    fun getObject() = object {
        val x: String = "x"
    }

    // The return type is A; x is not accessible
    fun getObjectA() = object: A {
        override fun funFromA() {}
        val x: String = "x"
    }

    // The return type is B; funFromA() and x are not accessible
    fun getObjectB(): B = object: A, B { // explicit return type is required
        override fun funFromA() {}
        val x: String = "x"
    }
}
```
- Singleton 패턴에 유용하다 : 스레드 세이프하고 최초 액세스시 초기화가 수행된다
```kotlin
object DataProviderManager {
    fun registerDataProvider(provider: DataProvider) {
        // ...
    }

    val allDataProviders: Collection<DataProvider>
        get() = // ...
}
```
- 자동으로 toString 생성됨
- data object 가능 : hash, equals 자동 생성(copy, componentN은 자동 생성 안됨)
```kotlin
data object MyDataObject {
    val x: Int = 3
}

fun main() {
    println(MyDataObject) // MyDataObject
}
```

# Companion Object
- 클래스 내부에서 companion 키워드로 표시
```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}
```
- 클래스 이름으로 간단하게 호출 가능
```kotlin
val instance = MyClass.create()
```
- 이름은 넣을 수도 있고 생략할 수 도 있음
```kotlin
class MyClass {
    companion object { }
}

val x = MyClass.Companion
```
- 클래스 멤버는 동반객체의 private에 접근할 수 있다
- 정적 멤버처럼 보여도 객체기 때문에 인터페이스 구현 가능
```kotlin
interface Factory<T> {
    fun create(): T
}

class MyClass {
    companion object : Factory<MyClass> {
        override fun create(): MyClass = MyClass()
    }
}

val f: Factory<MyClass> = MyClass
```
- @JvmStatic을 쓰면 실제 정적 메서드로 생성 가능 

# 객체 표현식과 선언 사이의 의미적 차이
- 객체 표현식은 사용되는 곳에서 즉시 실행 및 초기화 된다
- 객체 선언은 처음 접근할 때 초기화 된다
- 동반 객체는 해당 클래스 최초 로드시 초기화 된다