# 가시성 수정자(Visibility modifiers)
- 클래스, 객체, 인터페이스, 생성자, 함수, 속성, setter는 가시성 수정자를 가질 수 있다
- private : 선언된 곳에서만 사용 가능
- protected : private와 비슷하지만 + 상속받은 클래스에서도 사용 가능(클래스 내에서만 사용 가능)
- internal : 선언된 모듈에서 사용 가능
- public(디폴트) : 어디서나 사용 가능

# 패키지내 선언된 가시성 수정자
- protected는 사용 불가
```kotlin
// file name: example.kt
package foo

private fun foo() { ... } // visible inside example.kt

public var bar: Int = 5 // property is visible everywhere
    private set         // setter is visible only in example.kt

internal val baz = 6    // visible inside the same module
```

# 클래스내의 가시성 수정자
```kotlin
open class Outer {
    private val a = 1
    protected open val b = 2
    internal open val c = 3
    val d = 4  // public by default

    protected class Nested {
        public val e: Int = 5
    }
}

class Subclass : Outer() {
    // a is not visible
    // b, c and d are visible
    // Nested and e are visible

    override val b = 5   // 'b' is protected
    override val c = 7   // 'c' is internal
}

class Unrelated(o: Outer) {
    // o.a, o.b are not visible
    // o.c and o.d are visible (same module)
    // Outer.Nested is not visible, and Nested::e is not visible either
}
```

# 생성자
- 생성자에서 사용시 반드시 constructor 키워드를 명시
- sealed 클래스의 경우 protected가 기본이 된다
```kotlin
class C private constructor(a: Int) { ... }
```

# 지역선언
- 함수내부의 변수 등 지역 내부에서는 쓸수 없다

