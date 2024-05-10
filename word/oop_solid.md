
# 객체 지향 설계의 5원칙 S.O.L.I.D

- SRP(Single Responsibility Principle) : 단일 책임 원칙
- OCP(Open Closed Principle) : 개방 폐쇄 원칙
- LSP(Liskov Substitution Principle) : 리스코프 치환 법칙
- ISP(Interface Segregation Principle) : 인터페이스 분리 원칙
- DIP(Dependency Inversion Principle) : 의존관계 역전 법칙

# 단일 책임 원칙(SRP)
클래스는 1개의 책임만 가져야한다
> - 책임의 범위는 정해져 있지 않기 때문에 적절한 유연성을 가질 수 있다
> - 즉 한 클래스에 너무 많은 기능을 넣지 말라는 것

# 개방 폐쇄 원칙(OCP)
확장에는 열려 있고, 수정에는 닫혀 있어야 한다
> - **확장에 열려 있다** : 새로운 변경 사항이 발생했을 때 코드 추가가 쉬워야함
> - **변경에 닫혀 있다** : 새로운 변경 사항이 발생했을 때 기존 코드를 수정하지 않아도 모듈의 기능을 확장하거나 변경 할 수 있다

# 리스코프 치환 법칙(LSP)
상속 받은 자식 클래스(서브 타입)은 언제나 부모 클래스로 교체 할 수 있어야 한다
> - 대표적인 예로 Collection 인터페이스
> - 다른 클래스를 대입하더라도 동작에 문제가 없어야 한다
> - 예를 들어 ArrayList를 쓰다가 LinkedList로 바꾸고, 이를 다시 HashSet으로 바꾼다고 하여도 동작에 문제가 없어야됨

# 인터페이스 분리 원칙(ISP)
클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 된다
> 어떤 클래스를 이용하는 클라이언트가 여러 개이고, 이들이 해당 클래스의 특정 부분 집합만을 이용한다면, 이들을 따로 인터페이스 빼내어 클라이언트가 기대하는 메시지만을 전달 할 수 있도록 하는 것

# 의존관계 역전 법칙(DIP)
어떤 클래스를 사용할 상황이 생기면 직접적으로 그 클래스를 참조하지 않고 그 대상의 상위 요소(추상 클래스, 인터페이스)를 참조하라
> - 구현 클래스에 의존하지 말고 인터페이스에 의존하라
> - 만약 Linked List를 쓴다면, Collection이나 List를 참조하여 쓰는게 맞다

# 참고

|내용|URL|
|:---|:---|
|SOLID(객체 지향 설계)|https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84)|
|단일 책임 원|https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99|
|개방-폐쇄 원|https://ko.wikipedia.org/wiki/%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84_%EC%9B%90%EC%B9%99|
|리스코프 치환 원칙|https://ko.wikipedia.org/wiki/%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84_%EC%B9%98%ED%99%98_%EC%9B%90%EC%B9%99|
|인터페이스 분리 원칙|https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4_%EB%B6%84%EB%A6%AC_%EC%9B%90%EC%B9%99|
|의존관계 역전 법칙|https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84_%EC%97%AD%EC%A0%84_%EC%9B%90%EC%B9%99