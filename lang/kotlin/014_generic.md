# Generics
- 자바와 비슷하게 제네릭 타입을 선언 가능
- 유추 가능한 경우 변수 선언시 생략 가능
- 타입 검사는 컴파일 타임에만 이루어지고, 런타임 시간에는 타입 정보를 가지고 있지 않음(Foo<Bar>, Foo<Baz?>는 런타임에는 타입정보가 지워지기 때문에 동일하다 Foo<*>)
```kotlin
class Box<T>(t: T) {
    var value = t
}

val box: Box<Int> = Box<Int>(1)

val box = Box(1) // 1 has type Int, so the compiler figures out that it is Box<Int>

```

# Generics : out
- 반환값만 해당 타입을 쓸 수 있는 키워드
```kotlin
interface Source<out T> {
    fun nextT(): T
}

fun demo(strs: Source<String>) {
    val objects: Source<Any> = strs // This is OK, since T is an out-parameter
    // ...
}
```

# Generics : in
- 인자로만 해당 타입을 받을 수 있는 키워드
```kotlin
interface Comparable<in T> {
    operator fun compareTo(other: T): Int
}

fun demo(x: Comparable<Number>) {
    x.compareTo(1.0) // 1.0 has type Double, which is a subtype of Number
    // Thus, you can assign x to a variable of type Comparable<Double>
    val y: Comparable<Double> = x // OK!
}
```

# Generic 명시적 non-null 선언
```kotlin
interface ArcadeGame<T1> : Game<T1> {
    override fun save(x: T1): T1
    // T1 is definitely non-nullable
    override fun load(x: T1 & Any): T1 & Any // (T1 이면서 Any 타입이어야 하므로 Any 조건에 의해 T1이 nullable이더라도 반드시 non-null이 된다)
}
```
```kotlin
fun <T> example(input: T) where T : Any {
    // 이 함수는 nullable 타입을 받지 않음(where을 사용해 대체 가능)
    println(input)
}
```

# Generics : where
- 타입을 제한할 수 있는 키워드
```kotlin
fun <T> copyWhenGreater(list: List<T>, threshold: T): List<String>
    where T : CharSequence,
          T : Comparable<T> {
    return list.filter { it > threshold }.map { it.toString() }
}
```

# Generic 타입 검사
- 타입 정보가 런타임에는 삭제되므로 런타임에서 정확히 확인 하는 방법은 없다
```kotlin
if (something is List<*>) {
    something.forEach { println(it) } // The items are typed as `Any?`
}
```
- 컴파일 시간에 확인 가능하다면, 꺽쇠 없이도 스마트 캐스트가 동작한다
```kotlin
fun handleStrings(list: MutableList<String>) {
    if (list is ArrayList) {
        // `list` is smart-cast to `ArrayList<String>`
    }
}
```

# Generic 타입이 동일할 경우 _ 로 생략 가능
```kotlin
Underscore operator for type arguments﻿
The underscore operator _ can be used for type arguments. Use it to automatically infer a type of the argument when other types are explicitly specified:

abstract class SomeClass<T> {
    abstract fun execute() : T
}

class SomeImplementation : SomeClass<String>() {
    override fun execute(): String = "Test"
}

class OtherImplementation : SomeClass<Int>() {
    override fun execute(): Int = 42
}

object Runner {
    inline fun <reified S: SomeClass<T>, T> run() : T {
        return S::class.java.getDeclaredConstructor().newInstance().execute()
    }
}

fun main() {
    // T is inferred as String because SomeImplementation derives from SomeClass<String>
    val s = Runner.run<SomeImplementation, _>()
    assert(s == "Test")

    // T is inferred as Int because OtherImplementation derives from SomeClass<Int>
    val n = Runner.run<OtherImplementation, _>()
    assert(n == 42)
}
```

# reified
- inline 함수와 함께 사용하여 제네릭 타입의 인자를 기억하는 역할
> 코틀린의 제네릭 타입은 기본적으로 타입 소거(type erasure)라는 개념을 사용합니다. 이는 제네릭 타입 정보가 런타임에 존재하지 않는다는 것을 의미합니다. 예를 들어, 제네릭 타입 T가 런타임에는 실제 타입 정보를 가지지 않기 때문에, 이를 사용하여 특정 타입의 인스턴스를 생성하거나 타입을 검사하는 것이 불가능합니다.
> reified 키워드는 인라인 함수와 함께 사용되어, 컴파일 타임에 제네릭 타입 정보를 런타임에도 사용할 수 있게 합니다. 인라인 함수는 함수 호출을 함수 본문으로 대체하여 호출 오버헤드를 줄이는 방법입니다. reified 타입 파라미터는 인라인 함수 내에서 런타임 타입 검사와 같은 작업에 사용할 수 있습니다.
```kotlin
inline fun <reified T> printTypeName() {
    println(T::class.simpleName)
}

fun main() {
    printTypeName<String>() // Output: String
    printTypeName<Int>() // Output: Int
}
```
```kotlin
inline fun <reified T> createInstance(): T {
    return T::class.java.getDeclaredConstructor().newInstance()
}

fun main() {
    val instance: String = createInstance()
    println(instance) // Output: ""
}
```