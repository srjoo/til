# 데이터 클래스(Data class)
- 데이터를 보관하는데 주로 사용된다
- 각 데이터 클래스에 대해 컴파일러는 자동으로 인스턴스를 읽을 수 있는 출력으로 인쇄하고, 인스턴스를 비교하고, 인스턴스를 복사하는 등의 작업을 할 수 있는 추가 멤버 함수를 자동 생성(명시적으로 선언한 경우는 자동 생성하지 않는다)
```kotlin
data class User(val name: String, val age: Int)
```
- .equals() / .hashCode() : 기본 생성자의 변수만 비교하기 때문에 별도로 선언된 속성은 고려하지 않는다
- .toString() : "User(name=John, age=42)"
- .componentN() : 선언 순서대로 만들어짐
- .copy()기능
- 최소 한개의 변수가 있어야함
- 생성자의 모든 변수는 var 혹은 val 로 속성으로 선언해야함
- abstract, open, sealed, inner 선언 불가

# 별도 속성 선언시 주의
- 추가 멤버 함수는 기본 생성자의 변수만 비교하므로 주의해야한다
```kotlin
data class Person(val name: String) {
    var age: Int = 0
}

val person1 = Person("John")
val person2 = Person("John")
person1.age = 10
person2.age = 20

println("person1 == person2: ${person1 == person2}")
// person1 == person2: true

println("person1 with age ${person1.age}: ${person1}")
// person1 with age 10: Person(name=John)

println("person2 with age ${person2.age}: ${person2}")
// person2 with age 20: Person(name=John)
```

# copy 사용
- 일부 속성만 변경한 새로운 객체 만들때 유용
```kotlin
// 자동 구현된 copy 함수는 다음과 같다
// fun copy(name: String = this.name, age: Int = this.age) = User(name, age)

val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

# componentN을 활용한 구조분해
```kotlin
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age")
// Jane, 35 years of age
```