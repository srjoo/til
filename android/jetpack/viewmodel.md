# ViewModel
- Android의 생명 주기(OnCreate ~ OnDestroy)에 따라 데이터를 보관한다
- ViewModel의 `OnCleared()`는 onDestroy 다음에 불림
> 즉, finish가 호출 될 때 소멸 되므로 Activity 보다 생명 주기가 길다

# 장점
- 생명 주기에 영향을 받지 않고 데이터가 유지됨
- UI컨트롤러와 데이터가 분리된다
- 프래그먼트 간의 데이터 공유가 쉬워진다

# 주의할 점
- ViewModel은 Activity나 Context를 참조하면 안된다
> 수명주기가 길기 때문에 **Memory Leak**이 발생할 가능성이 있다
> 단, Application Context는 참조를 해도 괜찮다

# AndroidViewModel vs ViewModel
- AndroidViewModel은 Application 클래스를 포함한 Android 컴포넌트와 관련된 컨텍스트를 사용할 수 있다
> 따라서 Android의 시스템 서비스, 애플리케이션 수준의 데이터 관리는 AndroidViewModel을 사용하여야 하며, 단순 UI데이터는 ViewModel을 사용한다
- AndroidViewModel Context를 포함하기 때문에 테스트 코드 작성시 문제가 될 수 있다
> 테스트 코드에서 생명 주기와 관련된 코드를 만들면 안된다

그렇기 때문에 AndroidViewModel은 **애플리케이션 컨텍스트가 필요할 때만 사용해야 한다!**
또한 ViewModel 내에서 **액티비티나 액티비티를 참조하는 뷰에 대한 참조를 저장해서는 안 된다!**

# 사용법
1. ViewModelProvider 사용
2. by viewModels() 사용
3. by activityViewModels() 사용 (Fragment만 가능)

# gradle
```
implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.5.1'  // 1 필수
implementation 'androidx.activity:activity-ktx:1.6.1'              // 2 by viewModels()
implementation 'androidx.fragment:fragment-ktx:1.6.1'              // 3 by activityViewModels()
```

# ViewModelProvider
```kotlin
private lateinit var viewModel : SomeViewModel
viewModel = ViewModelProvider(activity or fragment).get(SomeViewModel::class.java)
```

> ViewModelStore은 ViewModel들은 HashMap 형태로 저장하고 있기에 SomeViewModel::class.java 를 Key값으로 사용해 ViewModel 객체를 가져온다. 이때 Key에 해당하는 Value가 없으면 생성하고 가져오므로 처음 ViewModel 객체를 만들더라도 get을 통해 가져올 수 있다

# by viewModels()
- 확장함수 ComponentActivity.viewModels()를 이용해서 코틀린에서 간단히 사용할 수 있다
- lateinit을 선언할 필요가 없다 (Lazy<T> 인터페이스 사용)
```kotlin
private val viewModel by viewModels<SomeViewModel>()
// cf
// private val viewModel = viewModels<SomeViewModel>().value
```

# by activityViewModels()
- Fragment.activityViewModels() 확장함수를 사용
- 내부적으로 Fragment가 속해 있는 Activity의 ViewModelStore로부터 ViewModel을 가져오므로 먼저 해당 Activity에 위의 2가지 방법 중 하나를 선택해 ViewModel을 선언해야한다
```kotlin
private val viewModel by activityViewModels<SomeViewModel>()
```