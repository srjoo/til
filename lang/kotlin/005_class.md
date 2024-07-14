# class
- 기본적으로 중괄호로 내용을 넣지만 본문이 없을 경우 생략 가능
```kotlin
class Person { /*...*/ }
class Empty
```
- 다음 사항이 포함될 수 있다 : 생성자, 초기화 블록, function, properties, nested and inner class, 객체 선언

# Constructors﻿
- 기본 생성자 + 하나 이상의 보조 생성자가 있을 수 있다
- 기본 생성자는 클래스 헤더에 선언(constructor 키워드는 생략 가능 / 가시성 키워드나 annotation이 없을 경우)
```kotlin
class Person constructor(firstName: String) { /*...*/ }
class Person(firstName: String) { /*...*/ }
class Customer public @Inject constructor(name: String) { /*...*/ }
```
- init 블럭으로 실행하려는 코드 사용 가능(여러개도 가능하다!)
- 초기화는 클래스 본문의 순서와 동일한 순서로 실행됨
- 기본 생성자는 초기화 블럭 및 변수 초기화시 사용 가능
```kotlin
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints $name")
    }
    
    val secondProperty = "Second property: ${name.length}".also(::println)
    
    init {
        println("Second initializer block that prints ${name.length}")
    }
}
```
- 단순 할당은 간편하게 가능
```kotlin
class Person(val firstName: String, val lastName: String, var age: Int)
```
- 기본값도 가능
```kotlin
class Person(val firstName: String, val lastName: String, var isEmployed: Boolean = true)
```
- trailing comma 허용
```kotlin
class Person(
    val firstName: String,
    val lastName: String,
    var age: Int, // trailing comma
) { /*...*/ }
```

# secondary constructors
- 내부에서 constructor로 선언 가능
```kotlin
class Person(val pets: MutableList<Pet> = mutableListOf())

class Pet {
    constructor(owner: Person) {
        owner.pets.add(this) // adds this pet to the list of its owner's pets
    }
}
```
- 기본 생성자가 있는 경우 기본 생성자를 호출해야함
```kotlin
class Person(val name: String) {
    val children: MutableList<Person> = mutableListOf()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```
- 기본 생성자가 없는 경우(public인 인자없는 기본 생성자가 자동 생성됨)에도 init 블럭은 먼저 실행된다
```kotlin
class Constructors {
    init {
        println("Init block")
    }

    constructor(i: Int) {
        println("Constructor $i")
    }
}
```

# abstract class
- 모든 기능을 구현하지 않아도됨
```kotlin
abstract class Polygon {
    abstract fun draw()
}

class Rectangle : Polygon() {
    override fun draw() {
        // draw the rectangle
    }
}
```
- 추상적이지 않은 open멤버를 추상화된 멤버로 재정의할 수 있다
```kotlin
open class Polygon {
    open fun draw() {
        // some default polygon drawing method
    }
}

abstract class WildShape : Polygon() {
    // Classes that inherit WildShape need to provide their own
    // draw method instead of using the default on Polygon
    abstract override fun draw()
}
```

# Companion object
- 클래스 인스턴스가 없어도 호출가능
- static 멤버가 없는 kotlin 특성 상 static 대신하여 많이 사용된다