# Compose 기본

- Compose를 사용하면 데이터를 받아서 UI 요소를 내보내는 _구성 가능한_ 함수 집합을 정의하여 사용자 인터페이스를 빌드할 수 있습니다
- UI 구성을 위한 함수는 반드시 `@Composable` annotation으로 지정되어야함. 컴파일러가 알아야 하기 때문
- `@Composable` 함수는 또 다른 `@Composable` 함수를 불러도 OK
- 리턴값은 필요없다
- `@Composable` 함수는 기본적으로 [멱등원](https://en.wikipedia.org/wiki/Idempotence#Computer_science_meaning)이어야함. (즉 동일한 데이터로 몇 번을 실행해도 한 번 실행한 것과 결과가 다르면 안됨)
- 사용자는 **뷰 개별로 getter, setter로 데이터를 설정**하지 않는다. 데이터가 변경되어 업데이트가 필요할 경우 `@Composable` 함수에 변경된 데이터를 넣어 UI를 업데이트한다


# 재구성 (Recomposition)

- 상태가 업데이트 되고 UI가 다시 그려지는 과정을 **재구성**이라고 한다
- 재구성을 할 때 데이터가 **변경**된 `@Composable` 함수만 호출되며, 변경되지 않은 경우 실행되지 않고 생략되니 이를 주의해야한다
	--  공유된 객체의 프로퍼티에 쓰기
	--  ViewModel의 관찰가능한 값을 업데이트 하기
	--  Shared Preference를 업데이트 하기
	-- 등등


# Compose의 특징

```kotlin
@Composable
fun ButtonRow() {
    MyFancyNavigation {
        StartScreen()
        MiddleScreen()
        EndScreen()
    }
}
```
- 위의 3개 중 어떤 함수가 먼저 실행되는가? **답은 알 수 없다**. 따라서 순서대로 실행될거라 기대하고 순서가 유의미한 함수를 만들면 안된다.
- 어떤 함수가 먼저 실행되나의 연장점으로 **함수들은 동시에 실행될 수 있다**, 즉 재구성은 백그라운드 스레드에서 실행된다
- `@Composable` 함수 컴파일러에 의해 재구성 될 수 있다. 따라서 **반드시 독립적이고 부작용이 없어야한다**
- `@Composable` 함수는 재구성시 매개변수의 변경 전에 완료될 거라 가정한다. 따라서 매우 매우 작업이 커서 업데이트 도중에 매개변수의 변경이 발생할 경우 취소하고 다시 재구성 하게 될 수 있다. 이 경우 **UI가 일관적이지 않은 상태**가 될 수 있으니 유의하자.
- **업데이트는 자주 실행될 수 있다**. 무거운 작업은 별도 스레드를 이용하자.

# Compose 시작

https://developer.android.com/develop/ui/compose/setup?hl=ko
> 최신 안드로이드 스튜디오 기준으로 Empty Activity를 선택하면 기본으로 설정되어 나오며, 버전이 계속 업데이트 되므로 링크로 대체한다

# 참고

|내용|URL|
|:---|:---|
|Compose의 이해|https://developer.android.com/develop/ui/compose/mental-model?hl=ko|
