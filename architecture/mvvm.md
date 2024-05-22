# MVVM(Model-View-ViewModel)
- MVVM 패턴은 MVC 패턴의 한계를 극복하기 위해 개발된 패턴
- Model : 어플리케이션의 데이터를 저장하는 역할을 담당한다
- View : 화면 구성을 담당하는 영역이다
- ViewModel : Model과 View 사이에서 중개자 역할(뷰와 1:N 매칭)
> Controller와 유사하지만, View에 직접 연결되지 않고 사용자 인터페이스를 통해 상호작용

# 구조
1. 사용자의 요청은 View를 통해 받게 된다.
2. View에서 요청을 받는다면, Command 패턴으로 View Model로 요청을 전달한다.
3. View Model은 Model에게 요청 처리에 필요한 데이터를 요청한다.
4. Model은 내부적으로 비즈니스 로직을 수행하여 View Model에게 필요한 데이터를 전달한다.
5. View Model은 전달받은 데이터를 가공하여 저장한다.
6. View는 View Model과 Data Binding하여 사용자에게 요청에 적절한 화면을 출력한다.
> MVVM 패턴은 Command 패턴과 Data Binding 패턴, 2가지 패턴을 활용하여 구현되었으며, 이 패턴들을 통해 View와 Model 사이에 연관되는 의존성을 제거

# 장점
- 컴포넌트의 명확한 역할 분리로 인해 서로간의 결합도를 낮출 수 있다.
- 코드의 재사용성 및 확장성을 높일 수 있다.
- 서비스를 유지보수하고 테스트하는데 용이해진다.

# 단점
- ViewModel 설계가 어렵다

# 참고

|내용|URL|
|:---|:---|
|[Android] 아키텍처 패턴 - MVC, MVP, MVVM|https://jionchu.tistory.com/13|
|여기도-MVC-저기도-MVC-MVC-패턴이-뭐야|https://velog.io/@langoustine/%EC%97%AC%EA%B8%B0%EB%8F%84-MVC-%EC%A0%80%EA%B8%B0%EB%8F%84-MVC-MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4-%EB%AD%90%EC%95%BC|