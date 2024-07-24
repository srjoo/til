# Inline value class
- 단일 속성을 가지고, 해당 속성을 래핑한다
- 런타임에는 실제 객체가 아닌 기본 타입으로 컴파일 된다
- 성능 최적화와 메모리 사용량 감소에 유리
- 컴파일 타임에 특정 타입을 가지므로 타입 안전성에 좋다
- 추상화, 클래스 상속 불가(인터페이스는 구현 가능, 언제나 final class이다)
```kotlin
// For JVM backends
@JvmInline
value class Password(private val s: String)


// No actual instantiation of class 'Password' happens
// At runtime 'securePassword' contains just 'String'
val securePassword = Password("Don't try this in production")
```
```kotlin
@JvmInline
value class Person(private val fullName: String) {
    init {
        require(fullName.isNotEmpty()) {
            "Full name shouldn't be empty"
        }
    }   

    constructor(firstName: String, lastName: String) : this("$firstName $lastName") {
        require(lastName.isNotBlank()) {
            "Last name shouldn't be empty"
        }
    }

    val length: Int
        get() = fullName.length

    fun greet() {
        println("Hello, $fullName")
    }
}

fun main() {
    val name1 = Person("Kotlin", "Mascot")
    val name2 = Person("Kodee")
    name1.greet() // the `greet()` function is called as a static method
    println(name2.length) // property getter is called as a static method
}
```

# 표현
- Int와 Integer의 관계처럼 래핑 관계와 매우 유사함
- 사용에 따라 자동으로 박싱되고 언박싱됨
- 따라서 참조동등성(메모리상 주소 같은지 비교)는 무의미함
```kotlin
interface I

@JvmInline
value class Foo(val i: Int) : I

fun asInline(f: Foo) {}
fun <T> asGeneric(x: T) {}
fun asInterface(i: I) {}
fun asNullable(i: Foo?) {}

fun <T> id(x: T): T = x

fun main() {
    val f = Foo(42)

    asInline(f)    // unboxed: used as Foo itself
    asGeneric(f)   // boxed: used as generic type T
    asInterface(f) // boxed: used as type I
    asNullable(f)  // boxed: used as Foo?, which is different from Foo

    // below, 'f' first is boxed (while being passed to 'id') and then unboxed (when returned from 'id')
    // In the end, 'c' contains unboxed representation (just '42'), as 'f'
    val c = id(f)
}
```

# Mangling
- 기본 타입으로 컴파일되므로 예상치 못한 충돌이 발생할 수 있다
```kotlin
@JvmInline
value class UInt(val x: Int)

// Represented as 'public final void compute(int x)' on the JVM
fun compute(x: Int) { }

// Also represented as 'public final void compute(int x)' on the JVM!
fun compute(x: UInt) { }
```
- 따라서 kotlin에서는 아래와 같이 변경하여 충돌이 해결됨(해시코드 삽입)
> fun compute(x: UInt)
> public final void compute-<hashcode>(int x)
- 자바코드에서 호출하려면 JvmName 어노테이션을 사용해야함
```kotlin
@JvmInline
value class UInt(val x: Int)

fun compute(x: Int) { }

@JvmName("computeUInt")
fun compute(x: UInt) { }
```

# 인라인 클래스 vs 별칭
- 둘다 런타임에는 기본 타입으로 표현
- 하지만 인라인 클래스는 할당시 같은 기본 타입이라도 오류가 발생하는 차이점이 있다
```kotlin
typealias NameTypeAlias = String

@JvmInline
value class NameInlineClass(val s: String)

fun acceptString(s: String) {}
fun acceptNameTypeAlias(n: NameTypeAlias) {}
fun acceptNameInlineClass(p: NameInlineClass) {}

fun main() {
    val nameAlias: NameTypeAlias = ""
    val nameInlineClass: NameInlineClass = NameInlineClass("")
    val string: String = ""

    acceptString(nameAlias) // OK: pass alias instead of underlying type
    acceptString(nameInlineClass) // Not OK: can't pass inline class instead of underlying type

    // And vice versa:
    acceptNameTypeAlias(string) // OK: pass underlying type instead of alias
    acceptNameInlineClass(string) // Not OK: can't pass underlying type instead of inline class
}
```

