# Hilt
- Hilt는 Android 클래스에 의존성 주입(DI)을 지원하고 수명 주기를 자동으로 관리해 주기 때문에 Android에서 DI를 사용하기에 적합한 라이브러리

# 기본 설정
- libs.versions.toml
```
[versions]
hilt = "2.41"

[libraries]
android-hilt = { group = "com.google.dagger", name = "hilt-android", version.ref = "hilt" }
android-hilt-compiler = { group = "com.google.dagger", name = "hilt-android-compiler", version.ref = "hilt" }

[plugins]
android-hilt = { id = "com.google.dagger.hilt.android", version.ref = "hilt" }
```

- 루트 프로젝트(build.gradle.kts)
```
plugins {
    alias(libs.plugins.android.hilt) apply false
}
```

- 앱 프로젝트(build.gradle.kts)
```
plugins {
    alias(libs.plugins.android.hilt)
    kotlin("kapt")
}

dependencies {
    implementation(libs.android.hilt)
    kapt(libs.android.hilt.compiler)
}
```

- Application 클래스
```kotlin
@HiltAndroidApp
class ExampleApplication : Application() { ... }
```

- AndroidManifest.xml
```
    <application
        android:name=".ExampleApplication"
```

- 종속성 주입할 클래스(예제는 Activity)
```kotlin
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() { ... }
```
> Application ('@HiltAndroidApp')
> ViewModel ('@HiltViewModel')
> Activity
> Fragment
> View
> Service
> BroadcastReceiver

- 의존성 주입 정의('@Inject')
```kotlin
class AnalyticsAdapter @Inject constructor(
  private val service: AnalyticsService
) { ... }
```
> constructor 키워드 필요

- 모듈 정의
> '@Module'와 '@InstallIn'을 사용한다.
> '@Binds'를 사용하여 인터페이스 인스턴스 삽입
```kotlin
interface AnalyticsService {
  fun analyticsMethods()
}

// Constructor-injected, because Hilt needs to know how to
// provide instances of AnalyticsServiceImpl, too.
class AnalyticsServiceImpl @Inject constructor(
  ...
) : AnalyticsService { ... }

@Module
@InstallIn(ActivityComponent::class)
abstract class AnalyticsModule {

  @Binds
  abstract fun bindAnalyticsService(
    analyticsServiceImpl: AnalyticsServiceImpl
  ): AnalyticsService
}
```
> '@InstallIn(ActivityComponent::class)'은 모든 Activity에서 사용할 수 있음을 의미한다

- '@Provides'를 사용하여 인스턴스 삽입
> 외부 모듈과 같이 소스가 없거나, 소스 수정을 원하지 않는 경우 '@Provides'를 사용하여 제공 방법을 정의한다
```kotlin
@Module
@InstallIn(ActivityComponent::class)
object AnalyticsModule {

  @Provides
  fun provideAnalyticsService(
    // Potential dependencies of this type
  ): AnalyticsService {
      return Retrofit.Builder()
               .baseUrl("https://example.com")
               .build()
               .create(AnalyticsService::class.java)
  }
}
```

- 의존성 주입 객체에게 context가 필요한 경우
> '@ApplicationContext'와 '@ActivityContext'를 제공하므로 필요한 것을 사용한다
```kotlin
class AnalyticsAdapter @Inject constructor(
    @ActivityContext private val context: Context,
    private val service: AnalyticsService
) { ... }
```

- Hilt 구성요소
|구성요소|Android 클래스|생성|제거|기본 Scope(생명주기)|
|---|---|---|---|---|
|ApplicationComponent|Application|Application.onCreate()|Application.onDestroy()|@Singleton|
|ActivityRetainedComponent|ViewModel|Activity.onCreate()|Activity.onDestroy()|@ActivityRetainedScope|
|ActivityComponent|Activity|Activity.onCreate()|Activity.onDestroy()|@ActivityScoped|
|FragmentComponent|Fragment|Fragment.onAttach()|Fragment.onDestroy()|@FragmentScoped|
|ViewComponent|View|View.super()|뷰 제거|@ViewScoped|
|ViewWithFragmentComponent|@WithFragmentBindings가 사용된 View|View.super()|뷰 제거|@ViewScoped|
|ServiceComponent|Service|Service.onCreate()|Service.onDestroy()|@ServiceScoped|

> Scope 사용 예(Activity 생명주기 동안 동일한 인스턴스 제공, 미지정시 매번 새로 생성)
```kotlin
@ActivityScoped
class AnalyticsAdapter @Inject constructor(
  private val service: AnalyticsService
) { ... }
```