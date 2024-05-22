# MVP(Model-View-Presenter)
- MVP 패턴 또한 MVC 패턴의 단점을 보완하기 위해 등장한 패턴으로, Model, View, Presenter로 구성되는 소프트웨어 아키텍처 패턴이다
- Model : 어플리케이션의 데이터를 저장하는 역할을 담당한다
- View : 화면 구성을 담당하는 영역이다
- Presenter : Model과 View 사이에서 중개자 역할(뷰와 1:1 매칭)
> Controller와 유사하지만, View에 직접 연결되지 않고 사용자 인터페이스를 통해 상호작용

# 구조
1. 사용자의 요청은 View를 통해 받게 된다.
2. View는 데이터를 Presenter에 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 요청을 통해 비즈니스 로직을 수행하여 Presenter에서 요청받은 데이터를 전달한다.
5. Presenter는 View에게 전달받은 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 출력한다.
> Presenter를 통해서만 데이터를 전달받기 때문에 MVC 패턴의 약점 중 하나인 View와 Model의 의존성을 제거

# 장점
- 컴포넌트의 명확한 역할 분리로 인해 서로간의 결합도를 낮출 수 있다.
- 코드의 재사용성 및 확장성을 높일 수 있다.
- 서비스를 유지보수하고 테스트하는데 용이해진다.

# 단점
- View-Presenter 사이의 의존성이 크다

# 참고

|내용|URL|
|:---|:---|
|[Android] 아키텍처 패턴 - MVC, MVP, MVVM|https://jionchu.tistory.com/13|
|여기도-MVC-저기도-MVC-MVC-패턴이-뭐야|https://velog.io/@langoustine/%EC%97%AC%EA%B8%B0%EB%8F%84-MVC-%EC%A0%80%EA%B8%B0%EB%8F%84-MVC-MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4-%EB%AD%90%EC%95%BC|