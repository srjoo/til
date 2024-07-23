# Enum Class
- 열거형 클래스는 주로 관련된 상수들을 모아두고, 이를 타입 안전하게 다루기 위해 사용됨
- 각 상수는 열거형 클래스의 객체로 간주되며, 고유한 이름을 가진다
```kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```
- enum class의 인스턴스이므로 생성자로 초기화 가능
```kotlin
enum class Color(val rgb: Int) {
    RED(0xFF0000),
    GREEN(0x00FF00),
    BLUE(0x0000FF)
}
```
- 모든 Enum Class는 Comparable을 자동 구현한다


# 자체 익명 클래스 선언 및 기본 함수 재정의 가능
```kotlin
enum class ProtocolState {
    WAITING {
        override fun signal() = TALKING
    },

    TALKING {
        override fun signal() = WAITING
    };

    abstract fun signal(): ProtocolState
}
```

# 인터페이스 구현 가능
```kotlin
enum class IntArithmetics : BinaryOperator<Int>, IntBinaryOperator {
    PLUS {
        override fun apply(t: Int, u: Int): Int = t + u
    },
    TIMES {
        override fun apply(t: Int, u: Int): Int = t * u
    };

    override fun applyAsInt(t: Int, u: Int) = apply(t, u)
}
```

# 사용예
- entries(코틀린 1.9에 추가됨) 혹은 values로 전체를 가져올 수 있다
- 각 enum은 name과 ordinal(인덱스 순서)를 가지고 있다 
```kotlin
enum class RGB { RED, GREEN, BLUE }

fun main() {
    for (color in RGB.entries) println(color.toString()) // prints RED, GREEN, BLUE
    println("The first color is: ${RGB.valueOf("RED")}") // prints "The first color is: RED"
}
```
- enumEntries<T>를 통해 제네릭한 enum 액세스를 구현 가능
```kotlin
enum class RGB { RED, GREEN, BLUE }

inline fun <reified T : Enum<T>> printAllValues() {
    println(enumEntries<T>().joinToString { it.name })
}

printAllValues<RGB>()
// RED, GREEN, BLUE
```