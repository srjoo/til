# MVI(Model-View-Intent)
- MVI는 reactive-programming으로 안드로이드 앱 개발을 하는 데 사용

# 역할
- Model : 상태, 다른 레이어와의 단방향 데이터 흐름을 보장하기 위해 변경이 불가능
- View : 하나 이상의 Activity나 Fragment로 구현
- Intent : 사용자 또는 앱내 발생하는 Action, 모든 Action에 대해 View는 Intent를 수신, 수신 후 ViewModel 혹은 Presenter는 Intent를 관찰하여 Model을 새로운 상태로 전환

# 전통적인 Model
- 데이터의 속성을 가진다
```kotlin
data class Movie(
  var voteCount: Int? = null,
  var id: Int? = null,
  var video: Boolean? = null,
  var voteAverage: Float? = null,
  var title: String? = null,
  var popularity: Float? = null,
  var posterPath: String? = null,
  var originalLanguage: String? = null,
  var originalTitle: String? = null,
  var genreIds: List<Int>? = null,
  var backdropPath: String? = null,
  var adult: Boolean? = null,
  var overview: String? = null,
  var releaseDate: String? = null
)
```
- MVP 모델 기준 Presenter의 역할은 이 데이터를 가지고 뷰를 제어하는 것
> 문제1. Multiple inputs: MVP와 MVVM에서, Presenter와 ViewModel은 많은 수의 입출력을 관리해야 하는 경우가 많습니다. 이건 많은 백그라운드 테스크가 있는 큰 앱에서는 문제를 일으킬 수 있습니다.
> 문제2. Multiple states: MVP와 MVVM에서, 비즈니스로직과 View는 언제든 다른 상태를 가질 수 있습니다. 개발자는 자주 Observable 과 Observer 콜백의 상태를 동기화시킵니다. 하지만 이건 행위의 충돌을 야기할 수 있습니다.
```kotlin
class MainPresenter(private var view: MainView?) {    
  override fun onViewCreated() {
    view.showLoading()
    loadMovieList { movieList ->
      movieList.let {
        this.onQuerySuccess(movieList)
      }
    }
  }
  
  override fun onQuerySuccess(data: List<Movie>) {
    view.hideLoading()
    view.displayMovieList(data)
  }
}
```

# MVI에서의 Model
- 현재 앱의 상태를 가르킴(상태이기 때문에 변경이 불가능하고 매번 새로운 상태를 만든다)
```kotlin
sealed class MovieState {
  object LoadingState : MovieState()
  data class DataState(val data: List<Movie>) : MovieState()
  data class ErrorState(val data: String) : MovieState()
  data class ConfirmationState(val movie: Movie) : MovieState()
  object FinishState : MovieState()
}
```

- MVP 모델 기준 Presenter는 상태를 전달하는 것
```kotlin
class MainPresenter {
  
  private val compositeDisposable = CompositeDisposable()
  private lateinit var view: MainView
  
  fun bind(view: MainView) {
    this.view = view
    compositeDisposable.add(observeMovieDeleteIntent())
    compositeDisposable.add(observeMovieDisplay())
  }
  
  fun unbind() {
    if (!compositeDisposable.isDisposed) {
      compositeDisposable.dispose()
    }
  }
  
  private fun observeMovieDisplay() = loadMovieList()
      .observeOn(AndroidSchedulers.mainThread())
      .doOnSubscribe { view.render(MovieState.LoadingState) }
      .doOnNext { view.render(it) }
      .subscribe()
}
```

# Model 불변(Immutable) 상태의 장점
- 단일 상태: 불변 데이터 구조는 매우 다루기 쉽고 한곳에서 관리되기 때문에 앱 내 모든 레이어 간 하나의 단일 상태를 보장
- Thread Safety: 이 부분은 RxJava나 LiveData와 같은 라이브러리를 사용하는 Reactive 앱에 특히 유용합니다. 어떤 함수도 모델을 수정할 수 없기 때문에 모델은 항상 한 곳에서 다시 만들어지고 유지

# MVI에서의 View
- 상태를 받아 View를 변환시킨다
```kotlin
class MainActivity : MainView {
  
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
  }
    
  //1
  override fun displayMoviesIntent() = button.clicks()
    
  //2
  override fun render(state: MovieState) {
    when(state) {
      is MovieState.DataState -> renderDataState(state)
      is MovieState.LoadingState -> renderLoadingState()
      is MovieState.ErrorState -> renderErrorState(state)
    }
  }
    
  //4
  private fun renderDataState(dataState: MovieState.DataState) {
      //Render movie list
  }
    
  //3
  private fun renderLoadingState() {
      //Render progress bar on screen
  }
	
  //5
  private fun renderErrorState(errorState: MovieState.ErrorState) {
      //Display error mesage
  }
}
```

# 상태는 어떻게 바꾸는가
- MVI에서 Model은 Immutable
- 따라서 상태의 무언가를 수정하는게 아니다
- 이전의 값을 참고하여 State Reducers를 하는 것
1. 앱의 새로운 상태를 나타내는 PartialState라는 새로운 상태
2. 앱의 이전 상태가 필요한 새로운 Intents가 있을때 완료된 상태로부터 새로운 PartialState 생성
3. reduce() 함수에서 이전 상태와 PartialState를 사용하여 화면에 표시할 새로운 상태로 병합
4. RxJava의 scan()을 사용하여 reduce() 함수가 앱의 초기 상태를 적용하고 새 상태를 반환
```kotlin
val myList = listOf(1, 2, 3, 4, 5)
var result = myList.reduce { accumulator, currentValue ->
  println("accumulator = $accumulator, currentValue = $currentValue")
  accumulator + currentValue }
println(result)
```

# 장점
- 데이터가 단방향으로 순환(일관성 있는 상태를 가지며, Thread 안정성 증가)
View -- 이벤트 --> Presenter -- 비즈니스 로직 --> 새로운 Model로 상태 변화 -- View에 State를 알림 --> View 변화 --> 반복

# 단점
- 학습 곡선이 크다
> RxJava, Multi Thread 등의 기반 지식이 필요