# package
- 소스 파일의 클래스와 함수와 같은 모든 내용이 패키지에 포함
- 아래 예제의 경우 org.example.printMessage 이고 org.example.Message이다
```kotlin
package org.example

fun printMessage() { /*...*/ }
class Message { /*...*/ }

// ...
```

# default import
kotlin.*
kotlin.annotation.*
kotlin.collections.*
kotlin.comparisons.*
kotlin.io.*
kotlin.ranges.*
kotlin.sequences.*
kotlin.text.*

JVM:
java.lang.*
kotlin.jvm.*

JS:
kotlin.js.*

# import
- 패키지의 특정 부분만
```kotlin
import org.example.Message // Message is now accessible without qualification
```
- 전부다 가져올 경우 와일드 카드 사용
```kotlin
import org.example.* // everything in 'org.example' becomes accessible
```
- 이름 충돌이 있는 경우 as로 다른 별칭으로 호출 가능
```kotlin
import org.example.Message // Message is accessible
import org.test.Message as TestMessage // TestMessage stands for 'org.test.Message'
```
- 함수나 클래스 뿐만 아니라 다른 것도 가능 : 최상위 함수 및 속성, 객체 선언 에 선언된 함수 및 속성, 열거형 상수