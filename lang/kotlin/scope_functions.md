# Scope functions(범위 함수)
- 코드 블록 실행이 목적인 함수
- 코드 블록 내부에서는 임시 범위(Scope)가 생성되고, 호출한 객체를 변수 이름 없이 접근가능

# 왜?
- 여러가지 작업들을 수행시, 특히 별다른 용도 없는 1회용 변수라고 할지라도 매번 변수이름을 타이핑해야 하고 역할을 기억해야 되며 네이밍까지 해야함
- 코드가 복잡해지면 코드 가독성이 하락해 유지보수가 어려워져 생산성이 하락
- 새로운 기능은 없지만, 범위 함수는 코드를 간결하고 읽기 쉽게 만듬

# let
- 코드 블럭 내부에서 it 으로 객체를 호출
- 코드 블럭의 마지막 값이 리턴 값
```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
val ret = numbers.let { 
    println("let 블럭1의 it == $it")
    it[0] // it(리스트)의 첫번째 요소(one) 리턴
}.let {
    println("let 블럭2의 it == $it")
    it[0] // it(String)의 첫번째 요소(o) 리턴
}
println("let 최종 결과 == $ret")

//let 블럭1의 it == [one, two, three, four, five]
//let 블럭2의 it == one
//let 최종 결과 == o
```

# run
- 코드 블럭 내부에서 this 로 객체를 호출
- 코드 블럭의 마지막 값이 리턴 값
- 객체 없이 호출 가능 (이 경우 this가 없다!)
```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
val ret = numbers.run {
	println("run with this == ${this}")
    this.size + size // this.size == size, this 이므로 블록 내부에서 생략이 가능하다!
}
run {
	println("run without this")
    // println("run without this == ${this}") this가 없으므로 컴파일 오류가 발생합니다
}
println("double size == ${ret}")

//run with this == [one, two, three, four, five]
//run without this
//double size == 10
```

# with
- 코드 블럭 내부에서 this 로 객체를 호출
- 코드 블럭의 마지막 값이 리턴 값
- this로 사용할 객체를 지정하여 호출할 수 있습니다
```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
val ret = with(numbers, {
	println("run with this == ${this}") // this는 numbers 입니다
    this.size + size // this.size == size, this 이므로 블록 내부에서 생략이 가능하다!
})
println("double size == ${ret}")

//run with this == [one, two, three, four, five]
//double size == 10
```

# apply
- 코드 블럭 내부에서 this 로 객체를 호출
- this 가 리턴 값
```kotlin
val newList = mutableListOf<String>().apply {
    add("one")
    add("two")
    add("three")
    add("four")
    add("five")
}
println("결과 : ${newList}")
	
//결과 : [one, two, three, four, five]
```

# also
- 코드 블럭 내부에서 it 으로 객체를 호출
- it 이 리턴 값
```kotlin
val newList = mutableListOf<String>().also {
    it.add("one")
    it.add("two")
    it.add("three")
    it.add("four")
    it.add("five")
}
println("결과 : ${newList}")

//결과 : [one, two, three, four, five]
```