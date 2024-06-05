# withIndex()
- iterator, array에서 인덱스, 값 모두를 추출함
```kotlin
val iterator = ('a'..'c').iterator()
for ((index, value) in iterator.withIndex()) {
	println("The element at $index is $value")
}
```
> The element at 0 is a
> The element at 1 is b
> The element at 2 is c

# asSequence()
자바에서 Stream API를 사용하기 위해서는
stream() 메소드를 이용해서 스트림으로 변환해야 합니다.
하지만 코틀린에서는 따로 스트림으로 변경하지 않아도 Stream API를 사용할 수 있도록
Iterable의 확장함수로 구현되어 있습니다.
컬렉션에 데이터가 많을 경우 asSequence()를 이용하여 시퀀스로 변경
데이터가 적을 경우 오히려 오버헤드가 커짐

# associate()
- Association 함수를 사용하면 Map 형식의 <Key, Value>타입으로 값들을 묶어줄 수 있다
- associateBy { it.name }
> it.name를 Key로, it = List의 Book원소를 Value로 Map을 만든다.
- associateBy( Book::name, Book::writer )
> Book의 name을 Key로서, Book의 writer를 Value로서 Map 형식으로 만들어 반환 해준다.
- associate ( it.writer to it.price )
> Book의 writer을 Key로서, Book의 price를 Value로서 Map 형식으로 만들어 반환 해준다.
```kotlin
data class Book(var name: String, var writer: String, var price: Int)

fun main(args: Array<String>) {
    val arr = mutableListOf<Book>()
    arr.add(Book("name1", "writer1", 12000))
    arr.add(Book("name2", "writer2", 21000))
    arr.add(Book("name3", "writer3", 31000))
    arr.add(Book("name4", "writer4", 28000))
    arr.add(Book("name4", "writer5", 23000))

    println("===============1================")
    var arr2 = arr.associateBy { it.name }
    arr2.forEach { println(it) }


    println("===============2================")
    arr2.forEach { println(it.value.writer) }


    println("===============3================")
    val arr3 = arr.associateBy(Book::name, Book::writer)    // key to value
    arr3.forEach { println(it) }


    println("===============4================")
    val arr4 = arr.associate { it.writer to it.price }
    arr4.forEach { println("key = ${it.key}, value = ${it.value}") }
}
```

# collection.indices
- for문 사용시 indices를 통해 범위를 표현할 수 있다
- indices는 IntRange를 반환한다
```kotlin
fun forLoop(){
    println("[for] 반복문")
    val items = listOf("apple", "banana", "kiwi")
    
    // A
    for(item in items) {
        println(item)
    }
    // B
    for(index in 0..(items.size-1)) {
        println("이건 item at $index is ${items[index]}")
    }
    // C
    for(index in 0 until items.size) {
        println("이건 item at $index is ${items[index]}")
    }
    // D
    for(index in items.indices) { //indices -> 0..2
        println("item at $index is ${items[index]}")
    }
}
```