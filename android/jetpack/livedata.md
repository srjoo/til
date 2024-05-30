# LiveData
- Android의 생명주기를 아는 데이터 타입
> OnStop 등 멈춰 있을 때 값의 업데이트가 일어나도 바로 업데이트 하지 않고 다시 OnResume 등 포어그라운드 상태로 변경될 때 마지막 값으로 업데이트 됨
- Observer 패턴을 따른다
> 데이터의 변경이 일어나면 콜백을 받아 처리함

**LiveData는 Databinding, ViewModel, RoomDatabase 등과 함께 쓰는 것이 일반적!**

# setValue vs postValue
- UI 스레드에서 바로 반영 vs 백그라운드 스레드에서 실행

# Databinding
- xml에서 데이터로 선언하여 바로 반영가능
- UI의 생명주기가 끝날 때 자동으로 해제되므로 rx의 dispose 와 같은 절차가 필요 없다

# Map
- LiveData의 변경을 다른 LiveData에게 알려준다
> 새로운 LiveData를 리턴하는게 아닌 데이터만 변경
```kotlin
val userLiveData:LiveData  = ...;
val userNameLLiveData  = Transformations.map(userLiveData, user -> {
     return user.firstName + " " + user.lastName; // String을 리턴합니다.
});
```

# SwitchMap
- SwitchMap은 LiveData를 변경사항을 받아서 다른 LiveData를 발행합니다. 일반적으로 RoomDatabase를 LiveData로 쓸때 많이 사용
> 새로운 LiveData를 리턴한다
```kotlin
val userIdLiveData:MutableLiveData = ...;
val userLiveData:LiveData = Transformations.switchMap(userIdLiveData, id ->
    repository.getUserById(id)); // LiveData를 리턴합니다.

fun setUserId(userId:String) {
     this.userIdLiveData.setValue(userId);
}
```

# MediatorLiveData
- 여러 데이터 소스를 한곳에서 Observe 할 때 사용
```kotlin
val liveData1:LiveData = ...;
val liveData2: LiveData = ...;

val liveDataMerger:MediatorLiveData = new MediatorLiveData<>();
 liveDataMerger.addSource(liveData1, value -> liveDataMerger.setValue(value));
 liveDataMerger.addSource(liveData2, value -> liveDataMerger.setValue(value));
 
 ```