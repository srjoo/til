
# 클린 아키텍처(Clean Architecture)
- 클린 아키텍처는 소프트웨어 시스템의 구조를 설계할 때 지켜야 할 원칙과 방법을 정의한 개념
> 소프트웨어가 가진 본연의 목적을 추구하려면 소프트웨어는 반드시 ‘부드러워’야 한다. 다시 말해 변경하기 쉬워야 한다. 이해관계자가 기능에 대한 생각을 바꾸면, 이러한 변경사항을 쉽게 적용할 수 있어야 한다. (로버트 C. 마틴, 클린 아키텍처)

# 원칙
- 의존성 역전 원칙(Dependency Inversion Principle)
> 고수준 모듈은 저수준 모듈에 의존해서는 안되며, 양쪽 모듈 모두 추상화에 의존해야 합니다. 이를 통해 느슨한 결합을 유지할 수 있습니다

- 경계(Boundary)의 분리
> 시스템을 여러 영역으로 나누고, 각 영역 사이의 인터페이스를 정의하여 각 영역의 독립성을 보장합니다

- 인터페이스 분리 원칙(Interface Segregation Principle)
> 클라이언트가 자신이 사용하지 않는 메서드에 의존하지 않아야 합니다. 즉, 인터페이스는 클라이언트의 요구에 딱 맞는 형태로 분리되어야 합니다

# 계층
- 프레젠터(Presenter) 또는 뷰모델(ViewModel) : 사용자 인터페이스(UI)와 비즈니스 로직 간의 중간 계층으로, UI에서 발생한 이벤트를 처리하고 필요한 데이터를 비즈니스 로직에 전달
- 유스케이스(Use Case) : 애플리케이션의 실제 비즈니스 로직을 포함하는 부분으로, 프레젠터나 뷰모델로부터 전달된 요청을 처리하고 데이터를 가공하여 반환
- 리포지토리(Repository) : 데이터베이스나 외부 데이터 원본과의 상호 작용을 담당하는 부분으로, 데이터를 가져오고 저장하는 작업을 수행합니다. 유스케이스는 리포지토리를 통해 데이터를 얻어옵니다
- 엔티티(Entity) : 애플리케이션의 핵심 데이터 구조를 나타내며, 데이터베이스나 네트워크에서 가져온 데이터를 객체로 변환한 형태

# 장점
- 유지보수 용이 : 계층마다 분리되어 있어서 각 계층을 변경해도 다른 계층에 영향을 주지 않는다
- 테스트 용이 : 의존성 주입으로 유닛 테스트 및 통합 테스트 쉽다
- 모듈간 분리 : 코드 재사용성 높아짐, 데이터베이스나 프레임워크 변경 쉽다

# 클린 아키텍처 + MVVM 아키텍처에 대한 아키텍처 구조
|Clean Architectrue|Presentation Layer|Domain Layer|Data Layer|
|:---:|:---:|:---:|:---:|
|MVVM|View, ViewModel|Model|Model|

# 참고

|내용|URL|
|:---|:---|
|왜 Android 신규 프로젝트는 클린 아키텍처를 도입하였는가|https://medium.com/cj-onstyle/android-%EB%B2%84%ED%8B%B0%EC%BB%AC-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%9D%98-%ED%81%B4%EB%A6%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EB%8F%84%EC%9E%85-a26d833e103c|