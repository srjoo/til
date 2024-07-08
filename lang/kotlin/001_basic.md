# 기본 구문
- 패키지 정의 : 가장 위에 선언되어야 함
- import
```kotlin
package my.demo

import kotlin.text.*
```
- main 함수 : 인자를 선언하지 않아도 되고, 선언할 경우 String Array를 받을 수 있다
```kotlin
fun main() {
    println("Hello world!")
}

fun main(args: Array<String>) {
    println(args.contentToString())
}
```
- 표준 출력 : print, println
- 표준 입력 : readln

# 함수
- fun NAME(args): ReturnType {}
```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```
- void 형 대신 Unit을 반환, 의미 없는 반환값(생략가능)
```kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
```

# 변수
- val : 한 번만 값이 할당되는 변수, 초기화 후 다른 값 할당 불가
- var : 재할당 가능한 변수
```kotlin
val x: Int = 5
// Declares the variable x and initializes it with the value of 5
var y: Int = 5
// Reassigns a new value of 6 to the variable y
y += 1
// 6
```

# 클래스
- class 키워드 사용
- 클래스의 속성은 선언이나 본문에 나열될 수 있다
- 클래스 선언에 나열된 매개변수가 있는 기본 생성자는 자동으로 사용 가능
```kotlin
class Rectangle(val height: Double, val length: Double) {
    val perimeter = (height + length) * 2
}
```
- 클래스 간 상속은 콜론( :)으로 선언
- 클래스는 기본적으로 final이며 명시적으로 open으로 선언해야 상속가능

# 주석
- 단일 //
- 멀티라인 /* */

# 문자열 템플릿
```kotlin
var a = 1
// simple name in template:
val s1 = "a is $a" 

a = 2
// arbitrary expression in template:
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

# 조건식
```kotlin
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}

fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

# for loop
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
for (item in items) {
    println(item)
}

val items = listOf("apple", "banana", "kiwifruit")
for (index in items.indices) {
    println("item at $index is ${items[index]}")
}
```

# while loop
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}
```

# when
- 스위치 비슷
```kotlin
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }
```

# Ranges
```kotlin
val x = 10
val y = 9
if (x in 1..y+1) {
    println("fits in range")
}

val list = listOf("a", "b", "c")

if (-1 !in 0..list.lastIndex) {
    println("-1 is out of range")
}
if (list.size !in list.indices) {
    println("list size is out of valid list indices range, too")
}

for (x in 1..5) {
    print(x)
}

for (x in 1..10 step 2) {
    print(x)
}
println()
for (x in 9 downTo 0 step 3) {
    print(x)
}
```

# Collections
```kotlin
for (item in items) {
    println(item)
}

when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}

val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
    .filter { it.startsWith("a") }
    .sortedBy { it }
    .map { it.uppercase() }
    .forEach { println(it) }
```

# Null 가능 값, Null 검사
- Nullable 유형 이름은 끝에 ?를 가진다
```kotlin
fun parseInt(str: String): Int? {
    // ...
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Using `x * y` yields error because they may hold nulls.
    if (x != null && y != null) {
        // x and y are automatically cast to non-nullable after null check
        println(x * y)
    }
    else {
        println("'$arg1' or '$arg2' is not a number")
    }    
}
```

# 유형 검사, 자동 캐스팅
- 유형 검사 후 암시적으로 해당 형으로 캐스팅 된다
```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

    // `obj` is still of type `Any` outside of the type-checked branch
    return null
}

fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}

fun getStringLength(obj: Any): Int? {
    // `obj` is automatically cast to `String` on the right-hand side of `&&`
    if (obj is String && obj.length > 0) {
        return obj.length
    }

    return null
}
```