# 확장(Extention)
- 상속 혹은 데코레이터 패턴과 같은 유형을 사용하지 않고 기능을 확장할 수 있다
- fun 클래스<타입(필요한 경우)>.swap(...) { ... }
```kotlin
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}

fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
```
- 실제 클래스에 들어가는게 아니라 호출시 연결할 뿐(static하게 연결되므로 런타임시 추가 비용 없다)
- 해당 클래스에 동일한 이름의 함수가 동일한 인자로 있다면 반드시 해당 클래스 내부의 함수가 호출된다(확장 불가)
```kotlin
class Example {
    fun printFunctionType() { println("Class method") }
}

fun Example.printFunctionType() { println("Extension function") }

Example().printFunctionType() // "Class method"
```

# Nullable receiver
- 인스턴스 없이 호출 가능한 확장기능도 만들 수 있다
```kotlin
fun Any?.toString(): String {
    if (this == null) return "null"
    // After the null check, 'this' is autocast to a non-nullable type, so the toString() below
    // resolves to the member function of the Any class
    return toString()
}
```

# 확장 속성
- 속성도 추가 가능(getter setter 명시적으로 정의해야함)
```kotlin
val <T> List<T>.lastIndex: Int
    get() = size - 1
```
- 클래스를 수정하지는 않으므로 백킹 필드를 가질 수 없다
```kotlin
val House.number = 1 // error: initializers are not allowed for extension properties
```

# 컴패니언 오브젝트 확장
- 컴패니언 오브젝트도 확장 가능하다
```kotlin
class MyClass {
    companion object { }  // will be called "Companion"
}

fun MyClass.Companion.printCompanion() { println("companion") }

fun main() {
    MyClass.printCompanion()
}
```

# 확장 스코프
- 패키지 바로 아래에 선언하는게 일반적
```kotlin
package org.example.declarations

fun List<String>.getLongestString() { /*...*/}
```
- 외부에서 사용시 import 해야함
```kotlin
package org.example.usage

import org.example.declarations.getLongestString

fun main() {
    val list = listOf("red", "green", "blue")
    list.getLongestString()
}
```

# 클래스 내부에서 확장 선언
- 클래스 내부에서도 확장을 선언할 수 있다
- 클래스 내부에서만 사용 가능
```kotlin
class Host(val hostname: String) {
    fun printHostname() { print(hostname) }
}

class Connection(val host: Host, val port: Int) {
    fun printPort() { print(port) }

    fun Host.printConnectionString() {
        printHostname()   // calls Host.printHostname()
        print(":")
        printPort()   // calls Connection.printPort()
    }

    fun connect() {
        /*...*/
        host.printConnectionString()   // calls the extension function
    }
}

fun main() {
    Connection(Host("kotl.in"), 443).connect()
    //Host("kotl.in").printConnectionString()  // error, the extension function is unavailable outside Connection
}
```
- 이름과 인자가 같은 경우 확장 클래스에 선언된 함수를 우선 호출한다(만약 클래스의 함수를 호출하려면 명시적으로 호출해야함)
```kotlin
class Connection {
    fun Host.getConnectionString() {
        toString()         // calls Host.toString()
        this@Connection.toString()  // calls Connection.toString()
    }
}
```
- 멤버로 선언된 확장 함수는 상속해서 override할 수 있다
```kotlin
open class Base { }

class Derived : Base() { }

open class BaseCaller {
    open fun Base.printFunctionInfo() {
        println("Base extension function in BaseCaller")
    }

    open fun Derived.printFunctionInfo() {
        println("Derived extension function in BaseCaller")
    }

    fun call(b: Base) {
        b.printFunctionInfo()   // call the extension function
    }
}

class DerivedCaller: BaseCaller() {
    override fun Base.printFunctionInfo() {
        println("Base extension function in DerivedCaller")
    }

    override fun Derived.printFunctionInfo() {
        println("Derived extension function in DerivedCaller")
    }
}

fun main() {
    BaseCaller().call(Base())   // "Base extension function in BaseCaller"
    DerivedCaller().call(Base())  // "Base extension function in DerivedCaller" - dispatch receiver is resolved virtually
    DerivedCaller().call(Derived())  // "Base extension function in DerivedCaller" - extension receiver is resolved statically
}
```