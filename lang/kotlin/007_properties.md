# 속성
- val : readonly
- var : read/write
```kotlin
class Address {
    var name: String = "Holmes, Sherlock"
    var street: String = "Baker"
    var city: String = "London"
    var state: String? = null
    var zip: String = "123456"
}
```
- 자동으로 setter getter가 만들어지므로 인스턴스.속성명 으로 간단히 접근할 수 있다
```kotlin
fun copyAddress(address: Address): Address {
    val result = Address() // there's no 'new' keyword in Kotlin
    result.name = address.name // accessors are called
    result.street = address.street
    // ...
    return result
}
```
- 사용자 정의 getter setter를 정의할 수 있다
```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
```
```kotlin
val area get() = this.width * this.height
```
```kotlin
var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value) // parses the string and assigns values to other properties
    }
```

# 컴파일 타임 상수
- 최상위 속성이거나 object선언 또는 동반 객체 의 멤버여야 함
- String 또는 기본 유형 으로 초기화해야 한다
- 사용자 정의 getter 불가

# lateinit var
- 나중에 초기화 가능한 변수(초기화가 되지 않고 접근할 경우 런타임 오류
```kotlin
public class MyTest {
    lateinit var subject: TestSubject

    @SetUp fun setup() {
        subject = TestSubject()
    }

    @Test fun test() {
        subject.method()  // dereference directly
    }
}
```