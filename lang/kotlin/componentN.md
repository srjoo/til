# componentN()
- 구조 분해를 사용하면 객체가 가지고 있는 여러 값을 분해해서 여러 변수에 한꺼번에 초기화할 수 있습니다.
- 데이터 클래스의 주 생성자에 들어있는 프로퍼티에 대해서는 컴파일러가 자동으로 componentN 함수를 만들어줍니다
- 구조 분해 선언은 함수의 리턴 값이 여러 개일 때 유용합니다.
- 반환할 모든 값이 들어갈 데이터 클래스를 정의하고 함수의 반환 타입을 이 데이터 클래스로 하면 된다
- 경우에 따라서 모든 값이 필요하지 않다면 사용되지 않는 값은 밑줄(_) 로 대체할 수도 있습니다
- 이와 비슷한 기능으로는 코틀린 표준 라이브러리에 미리 정의된 Pair 나 Triple 클래스를 사용하면 함수에서 여러 값을 더 간단하게 반환할 수 있습니다.

```kotlin
val car = Car("현대", "그랜저", "그랜저 IG")
val (manufacturer, _, model) = car

>>> println("제조사 : ${manufacturer}, 모델 : ${model}")
```

# 일반 클래스 구조 분해 예제
```kotlin
/* 일반 클래스에서 구조 분해 선언하는 예제 */
class Car(val manufacturer: String, val model: String) {
   operator fun component1() = manufacturer
   operator fun component2() = model
}

/* 구조 분해 간단한 예제 */
>>> val car = Car("현대", "그랜저 IG")
>>> val (manufacturer, model) = car  //변수를 선언함과 동시에 car의 여러 컴포넌트로 초기화 합니다.
>>> println(manufacturer)
현대
>>> println(model)
그랜저 IG
```

```kotlin
/* 구조 분해 선언을 사용해 Map 이터레이션하기 */
val map = mapOf("JetBrains" to "Kotlin")

for((key, value) in map) {  //루프 변수에 구조 분해 선언을 사용
   println("${key} -> ${value}")
}

>>> JetBrains -> Kotlin
```

```kotlin
/* 구조 분해 선언을 사용해 List 이터레이션하기 */
val list = listOf("Android", "Kotlin", "Java")

for((index, element) in list.withIndex()) {  //루프 변수에 구조 분해 선언을 사용
   println("${index} -> ${element}")
}

>>> 0 -> Android
>>> 1 -> Kotlin
>>> 2 -> Java
```