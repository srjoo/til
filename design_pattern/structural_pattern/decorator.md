
# 데코레이터(Decorator)

-  객체의 결합을 통해 기능을 동적으로 유연하게 확장 할 수 있게 해주는 패턴
> 즉, 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의 한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계 하는 방식이다.

# 기본 구조
- 기본 인터페이스 Component가 있고, 이를 구현한 ConcreteComponent가 있다
- Decorator는 Component를 상속 받고, ConcreteComponent를 받아 ConcreteComponent 기능을 수행한다
- ConcreteDecorator는 실제적인 기능의 추가를 한다

# 장점
- 기존 코드를 수정하지 않아도 확장이 자유롭다
- 함수 패턴 유지 : 기존 함수의 시그니처(함수명, 매개변수 목록, 반환값) 수정 없이 기능 추가 가능
- 재사용성, 모듈화

# 단점
- 코드만으로 기능이 어떻게 동작하는지 이해가 어렵다
- 런타입 생성 객체의 구조를 알아야함
> 데코레이터 패턴은 적용 순서에 따라 결과값이 달라질 수 있기 때문에 런타임에 어떻게 사용되는지 알아야 한다

# 예제
버스 요금을 계산하는 예제를 만들어보자
기본 요금은 1000원이면 아래와 같이 쉽게 구현이 가능할 것이다
```java
interface Fare {
	int calcFare();
}

class BaseFare implements Fare {
	public int calcFare() {
		return 1000;
	}
}
```
여기에 거리에 따른 요금 1km 당 100원, 우대 할인 50%를 추가한다면 어떻게 해야 할까? 데코레이터 패턴을 쓰면 아래와 같이 구현이 가능하다
```java
abstract class FareDecorator implements Fare {
	protected final Fare target;
	protected Decorator(Fare target) {
		this.target = target;
	}

	protected int delegate() {
		return target.calculate();
	}  
}

class DistanceFare extends FareDecorator {
	private int distance;
	public DistanceFare(Fare target, int distance) {
		super(target);
		this.distance = distance;
	}
	
	public int calcFare() {
		return delegate() + (distance * 100);
	}
}

class DiscountFare extends FareDecorator {
	public DistanceFare(Fare target) {
		super(target);
	}
	
	public int calcFare() {
		return (int)(delegate() * 0.5);
	}
}

void main() {
	int distance = 20;
	int normalFare = new DistanceFare(new BaseFare(), distance).calcFare();
	int discountFare = new DiscountFare(DistanceFare(new BaseFare(), distance)).calcFare();
	System.out.println(distance + "km 이동시 기본 요금은 " + normalFare + "원 입니다");
	System.out.println(distance + "km 이동시 할인 요금은 " + discountFare + "원 입니다");
}
```
> 이렇게 기본 BaseFare의 수정 없이 거리 요금과 할인 요금을 모두 구현했고, 할인 요금을 적용하고 적용 안하는 것도 런타임시 결정될 수 있다.
> 거리 요금을 먼저 적용하느냐, 할인 요금을 먼저 적용하느냐에 따라 결과 값이 다를 수 있다는 점을 생각해야한다
> 따라서 런타임시 어떤 순서로 실행되는지 알지 못한다면 소스 코드만 보고 결과 값을 예측하기는 어렵다

# 참고

|내용|URL|
|:---|:---|
|데코레이터 패턴|https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0_%ED%8C%A8%ED%84%B4|
|데코레이터 패턴: 구현과 한계|https://bugoverdose.github.io/development/decorator-pattern-implementation-and-limitations/|