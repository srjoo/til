# if
- 코틀린에서 if는 표현식이므로 값 리턴 가능
```kotlin
max = if (a > b) a else b
```
```kotlin
val maxLimit = 1
val maxOrLimit = if (maxLimit > a) maxLimit else if (a > b) a else b
```
- 블럭으로도 사용 가능하다
```kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```
- 단 값을 리턴할 때는 반드시 else 구문이 필요

# when
- 여러 분기가 있는 조건식을 정의
- C, Java의 switch와 비슷
```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```
- 표현식으로 사용가능하므로 값 리턴 가능하다
```kotlin
enum class Bit {
    ZERO, ONE
}

val numericValue = when (getRandomBit()) {
    Bit.ZERO -> 0
    Bit.ONE -> 1
    // 'else' is not required because all cases are covered
}
```	
- else 케이스가 필수인 경우 : Boolean, enum, sealed
```kotlin
fun isTrue(): Boolean? {
    // 예제 함수로, Boolean 값을 반환합니다.
    return true
}

// Boolean의 모든 경우를 다루는 경우 'else'가 필요하지 않음
when (isTrue()) {
    true -> println("It's true")
    false -> println("It's false")
    null -> println("It's null")
    // 모든 경우가 다뤄졌으므로 'else' 분기가 필요하지 않습니다.
}

// 일부 경우만 다루는 경우 'else'가 필요함
when (isTrue()) {
    true -> println("It's true")
    false -> println("It's false")
    else -> println("It's null or undefined")
    // null의 경우를 다루지 않았으므로 'else' 분기가 필요합니다.
}
```
```kotlin
enum class Color {
    RED, GREEN, BLUE
}

fun getColor(): Color {
    // 예제 함수로, Color 중 하나를 반환합니다.
    return Color.RED
}

// 모든 경우를 다루는 경우 'else'가 필요하지 않음
when (getColor()) {
    Color.RED -> println("red")
    Color.GREEN -> println("green")
    Color.BLUE -> println("blue")
    // 모든 경우가 다뤄졌으므로 'else' 분기가 필요하지 않습니다.
}

// 일부 경우만 다루는 경우 'else'가 필요함
when (getColor()) {
    Color.RED -> println("red")
    else -> println("not red")
    // GREEN과 BLUE의 경우를 다루지 않았으므로 'else' 분기가 필요합니다.
}
```
```kotlin
sealed class Expr {
    data class Const(val number: Double) : Expr()
    data class Sum(val e1: Expr, val e2: Expr) : Expr()
    object NotANumber : Expr()
}

fun eval(expr: Expr): Double = when (expr) {
    is Expr.Const -> expr.number
    is Expr.Sum -> eval(expr.e1) + eval(expr.e2)
    Expr.NotANumber -> Double.NaN
    // 모든 경우가 다뤄졌으므로 'else' 분기가 필요하지 않습니다.
}
```
- 여러 사례를 정의하는 경우 ,를 쓴다
```kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```
- 범위로 표현 가능하다
```kotlin
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```
- 타입 검사도 가능(스마트 캐스트 동작함)
```kotlin
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```
- if문 대용으로도 사용 가능
```kotlin
when {
    x.isOdd() -> print("x is odd")
    y.isEven() -> print("y is even")
    else -> print("x+y is odd")
}
```
- 변수 정의 가능하다
```kotlin
fun Request.getBody() =
    when (val response = executeRequest()) {
        is Success -> response.body
        is HttpError -> throw HttpException(response.status)
    }
```

# for in
- collection
```kotlin
for (item in collection) print(item)
```
- range
```kotlin
for (i in 1..3) {
    println(i)
}
for (i in 6 downTo 0 step 2) {
    println(i)
}
```
- array
```kotlin
for (i in array.indices) {
    println(array[i])
}
```
- array (with index)
```kotlin
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

# while, do while
- 다른 언어와 동일

# return
- 가장 가까운 함수나 익명 함수에서 떠난다
- 표현식에서 Nothing으로 표현할 수 있다
```kotlin
val s = person.name ?: return
```

# break
- 가장 가까운 루프 종료(for, while)

# continue
- 가장 가까운 루프의 다음 단계로

# label
- 모든 표현식은 이름을 붙일 수 있다
- NAME@으로 가능
```kotlin
loop1@ for (i in 1..100) {
    for (j in 1..100) {
        if (...) break@loop1 // 해당 레이블이 표시된 루프 바로 뒤의 실행 지점으로 점프
    }
}
```
- 단순 함수안의 블록에서 리턴을 쓰면 해당 함수가 종료된다
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return // non-local return directly to the caller of foo()
        print(it)
    }
    println("this point is unreachable")
}
```
- label을 지정해서 회피 가능
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit // local return to the caller of the lambda - the forEach loop
        print(it)
    }
    print(" done with explicit label")
}
```
- 암묵적 label : 함수 이름
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach // local return to the caller of the lambda - the forEach loop
        print(it)
    }
    print(" done with implicit label")
}
```
- 반환값이 있을 경우 : return@loop value 형태로 반환
```kotlin
fun main() {
    val result = calculateSum()
    println(result) // 24
}

fun calculateSum(): Int {
    val sum = listOf(1, 2, 3, 4, 5).sumBy {
        if (it == 3) return@sumBy 0 // 람다 내에서 특정 값을 반환
        it * 2
    }
    return sum // 람다에서 반환된 값들을 합산한 결과
}
```

# try catch, throw
- 모든 예외는 Throwable을 상속
- throw 인스턴스 형태
```kotlin
throw Exception("Hi There!")
```
- throw는 try catch로 받을 수 있다(catch와 finally는 생략 가능)
```kotlin
try {
    // some code
} catch (e: SomeException) {
    // handler
} finally {
    // optional finally block
}
```
- try도 표현식이기 때문에 값을 전달 할 수 있다
```kotlin
val a: Int? = try { input.toInt() } catch (e: NumberFormatException) { null }
```

# Nothing
- Nothing 타입은 반환되지 않는 코드 경로를 표시하는 데 사용
- throw 표현식은 Nothing 타입을 가지며, 예외를 던지는 함수는 Nothing 타입을 반환할 수 있다
- Nothing?는 null 값을 가질 수 있는 Nothing 타입의 변형
-
```kotlin
fun fail(message: String): Nothing {
    throw IllegalArgumentException(message)
}

val s = person.name ?: fail("Name required")
println(s) // 's'는 이 시점에서 초기화된 것으로 간주됩니다.(컴파일시 에러가 발생하지 않는다)
```