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