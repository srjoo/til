# Numbers
- JVM에서의 숫자 표현 : 기본 유형으로 저장된다(int, double등), null이 가능한 숫자 참조(Int? 등)를 만들거나 제네릭을 사용하는 경우, 박싱 된다(Double, Integer 등)
```kotlin
val a: Int = 100
val boxedA: Int? = a
val anotherBoxedA: Int? = a

val b: Int = 10000
val boxedB: Int? = b
val anotherBoxedB: Int? = b

println(boxedA === anotherBoxedA) // true
println(boxedB === anotherBoxedB) // false
```
```kotlin
// Hypothetical code, does not actually compile:
val a: Int? = 1 // A boxed Int (java.lang.Integer)
val b: Long? = a // Implicit conversion yields a boxed Long (java.lang.Long)
print(b == a) // Surprise! This prints "false" as Long's equals() checks whether the other is Long as well
```
- 모든 숫자 유형은 다른 유형으로 전환하는 함수를 지원한다
toByte(): Byte
toShort(): Short
toInt(): Int
toLong(): Long
toFloat(): Float
toDouble(): Double

- 많은 경우 컨텍스트에서 유추되고, 산술 연산시 적절한 변환이 일어나므로 명시적 변환이 필요 없을 때가 많다
```kotlin
val l = 1L + 3 // Long + Int => Long
```

# Integer types
- Byte : 1byte, -128~127
- Short : 2bytes, -32768~32767
- Int : 4bytes, -2,147,483,648~2,147,483,647
- Long : 8bytes, -9,223,372,036,854,775,808~9,223,372,036,854,775,807
```kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```

# Unsigned integer types
- UByte : 1byte, 0~255
- UShort : 2bytes, 0~65,535
- UInt : 4bytes, 0~4,294,967,295
- ULong : 8bytes, 0~18,446,744,073,709,551,615

# Floating-point types
- Float : 4bytes
- Double : 8bytes
```kotlin
val pi = 3.14 // Double
// val one: Double = 1 // Error: type mismatch
val oneDouble = 1.0 // Double
val e = 2.7182818284 // Double
val eFloat = 2.7182818284f // Float, actual value is 2.7182817
```

# 리터럴 상수
- 10진수(Decimals): 123
- Long 타입: 123L
- 16진수(Hexadecimals): 0x0F
- 2진수(Binaries): 0b00001011
- 숫자 상수를 더 읽기 쉽게 하기 위해 밑줄(_)을 사용할 수 있다
```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```
- 부호 없는 정수는 u, U로 표현한다
```kotlin
val b: UByte = 1u  // UByte, expected type provided
val s: UShort = 1u // UShort, expected type provided
val l: ULong = 1u  // ULong, expected type provided

val a1 = 42u // UInt: no expected type provided, constant fits in UInt
val a2 = 0xFFFF_FFFF_FFFFu // ULong: no expected type provided, constant doesn't fit in UInt
val a = 1UL // ULong, even though no expected type provided and constant fits into UInt
```

# 비트연산
- shl : 부호 있는 비트이동(왼쪽)
- shr : 부호 있는 비트이동(오른쪽)
- ushr : 부호 있는 비트이동(오른쪽)
- and : 두 숫자의 각 비트가 둘다 1이면 1
- or : 두 숫자의 각 비트가 하나라도 1이면 1
- xor : 두 숫자의 각 비트가 다를 때 해당 비트는 1이 된다
- inv : 숫자의 모든 비트를 반전

# 부동 소수점 숫자에 대한 비교 연산
- 동등 : a == b, a != b
- 비교 : a < b, a > b, a <= b, a >= b
- 범위 비교 : a..b, x in a..b, x !in a..b
- NaN은 자신과 같다고 간주됩니다.
- NaN은 POSITIVE_INFINITY를 포함한 다른 모든 요소보다 크다고 간주됩니다.
- (-0.0)은 0.0보다 작다고 간주됩니다.

# Booleans
- true or false
- 비교연산자 사용 가능(|| &&)

# Character
- 'c' 와 같이 작은 따옴표로 표기
- 특수 문자는 역슬래시로 시작
- 유니코드는 \u코드 로 나타낸다

# String
- "string" 와 같이 큰 따옴표로 표기
- for in 사용가능
```kotlin
for (c in str) {
    println(c)
}
```

# String 리터럴
- 이스케이프된 문자열 : 이스케이핑은 기존 다른 언어들과 동일하게 백슬래시로 한다
```kotlin
val s = "Hello, world!\n"
```
- 멀티라인 문자열 : 쌍따옴표 3개로 멀티라인을 표현할 수 있다
```kotlin
val text = """
    for (c in "foo")
        print(c)
"""
val text2 = """
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
    """.trimMargin()
```
> trimMargin 함수를 이용해서 여백을 제거할 수 있다(일반적으로 여백 구분에 | 사용)

# String 템플릿
- $표시로 표기하며 toString 함수가 호출된다
```kotlin
val i = 10
println("i = $i") 
// i = 10

val letters = listOf("a","b","c","d","e")
println("Letters: $letters") 
// Letters: [a, b, c, d, e]

val s = "abc"
println("$s.length is ${s.length}") 
// abc.length is 3
```

# String 포맷
- 특정 요구 사항에 맞게 문자열의 형식을 지정
```kotlin
// Formats an integer, adding leading zeroes to reach a length of seven characters
val integerNumber = String.format("%07d", 31416)
println(integerNumber)
// 0031416

// Formats a floating-point number to display with a + sign and four decimal places
val floatNumber = String.format("%+.4f", 3.141592)
println(floatNumber)
// +3.1416

// Formats two strings to uppercase, each taking one placeholder
val helloString = String.format("%S %S", "hello", "world")
println(helloString)
// HELLO WORLD

// Formats a negative number to be enclosed in parentheses, then repeats the same number in a different format (without parentheses) using `argument_index$`.
val negativeNumberInParentheses = String.format("%(d means %1\$d", -31416)
println(negativeNumberInParentheses)
//(31416) means -31416
```

# Array
- 특별한 일이 없다면 배열보다 컬렉션을 사용하라
- 컬렉션은 읽기 전용으로 설정할 수 있고 크기도 자유롭게 변경 가능하다
- 컬렉션은 비교 연산이 가능하지만 배열은 그렇지 않다
- 배열 생성 : arrayOf(), arrayOfNulls(), emptyArray()
```kotlin
// Creates an array with values [1, 2, 3]
val simpleArray = arrayOf(1, 2, 3)
println(simpleArray.joinToString())
// 1, 2, 3

// Creates an array with values [null, null, null]
val nullArray: Array<Int?> = arrayOfNulls(3)
println(nullArray.joinToString())
// null, null, null
```
- 다차원 배열
```kotlin
// Creates a two-dimensional array
val twoDArray = Array(2) { Array<Int>(2) { 0 } }
println(twoDArray.contentDeepToString())
// [[0, 0], [0, 0]]

// Creates a three-dimensional array
val threeDArray = Array(3) { Array(3) { Array<Int>(3) { 0 } } }
println(threeDArray.contentDeepToString())
// [[[0, 0, 0], [0, 0, 0], [0, 0, 0]], [[0, 0, 0], [0, 0, 0], [0, 0, 0]], [[0, 0, 0], [0, 0, 0], [0, 0, 0]]]
```
- 배열 요소 접근은 []를 사용
```kotlin
val simpleArray = arrayOf(1, 2, 3)
val twoDArray = Array(2) { Array<Int>(2) { 0 } }

// Accesses the element and modifies it
simpleArray[0] = 10
twoDArray[0][0] = 2

// Prints the modified element
println(simpleArray[0].toString()) // 10
println(twoDArray[0][0].toString()) // 2
```
- 스프레드 연산자 : 인자에 * 를 넣어 가변적인 수의 인수를 포함하는 배열을 전달할 수 있다
```kotlin
fun main() {
    val lettersArray = arrayOf("c", "d")
    printAllStrings("a", "b", *lettersArray)
    // abcd
}

fun printAllStrings(vararg strings: String) {
    for (string in strings) {
        print(string)
    }
}
```
- 배열 내용 비교 : .contentEquals() 함수를 써라. == 과 같은 연산자는 사용할 수 없다
```kotlin
val simpleArray = arrayOf(1, 2, 3)
val anotherArray = arrayOf(1, 2, 3)

// Compares contents of arrays
println(simpleArray.contentEquals(anotherArray))
// true

// Using infix notation, compares contents of arrays after an element 
// is changed
simpleArray[0] = 10
println(simpleArray contentEquals anotherArray)
// false
```
- 배열 변환 : 합(sum, 숫자형에서만 사용가능), 혼합(shuffle), toList, toSet, toMap(pair로 지정된 경우만 가능)
```kotlin
val pairArray = arrayOf("apple" to 120, "banana" to 150, "cherry" to 90, "apple" to 140)

// Converts to a Map
// The keys are fruits and the values are their number of calories
// Note how keys must be unique, so the latest value of "apple"
// overwrites the first
println(pairArray.toMap())
// {apple=140, banana=150, cherry=90}
```

# type 체크
- is, !is 연산자를 사용
- 조건문에서 검사할 경우 내부에서 스마트 캐스트가 일어난다
```kotlin
fun demo(x: Any) {
    if (x is String) {
        print(x.length) // x is automatically cast to String
    }	
}

fun demo2(x: Any) {
	if (x !is String) return
		print(x.length) // x is automatically cast to String
}
```
- when이나 while에도 스마트 캐스트가 일어난다
```kotlin
when (x) {
    is Int -> print(x + 1)
    is String -> print(x.length + 1)
    is IntArray -> print(x.sum())
}
```

# type 캐스팅
- as : 안전하지 않다
```kotlin
val x: String = y as String
```
- as? : 안전하다
```kotlin
val x: String? = y as? String
```
