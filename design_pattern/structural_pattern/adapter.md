
# 어댑터(Adapter)

- 호환되지 않는 인터페이스를 가진 객체를 사용자가 기대하는 다른 인터페이스로 변환하는 패턴
> 예를 들어 어떠한 객체A가 결과 값으로 XML 데이터를 주고, 객체B는 입력 값으로 JSON 데이터가 필요하다면, 중간에서 XML_TO_JSON_ADAPTER 를 구현함으로 두 개의 객체를 동작하도록 할 수 있다

> 어댑터라는 이름은 어딘가 익숙하지 않은가? 일본 여행을 가면 돼지코라고 불리는 어댑터로 전자제품의 플러그 모양을 변경하는 것을 생각하면 된다

# 장점
- 단일책임 원칙 : 객체A와 객체B 사이의 인터페이스 변환 코드를 분리할 수 있다
- 개방폐쇄 원칙 : 기존 코드에 영향이 없이 두 클래스를 연결 할 수 있다

# 단점
- 어댑터 클래스를 구현하기 위한 코드를 별도로 구현해야 한다
> 만약 두 클래스를 변경해도 문제가 되지 않는다면 클래스를 변경하는게 더 간단할 수도 있다

# 다른 패턴과 차이점
- 브리지 : 사전에 설계되기 때문에 독립적인 개발이 가능하도록 만들지만, 어댑터는 **기존 어플리케이션** 함께 사용가능하고, 호환되지 않는 객체들 잘 동작되게 만든다
- 데코레이터 : 데코레이터는 **기존 인터페이스의 확장**이지만 어댑터는 새로운 인터페이스이다
- 프록시 : 인터페이스가 기존과 **동일**하다
- 파사드 : 기존 객체의 **새로운 인터페이스**를 만든다. 하지만 파사드는 완전히 새로운 인터페이스를 지향하며, 어댑터는 기존 인터페이스를 활용하는데 중점을 둔다. 그리고 일반적으로 어댑터는 **한 객체**만 래핑한다.

# 예제

```java
interface JPPlugHole {
	JPPlug providePlug();
}

interface KRProduct {
	KRPlug getPlug();
}

class KRToJPAdapater implements JPPlugHole {
	private final KRProduct product;
	public KRToJPAdapater(KRProduct product) {
		this.product = product;
	}

	@override
	public JPPlug providePlug() {
		KRPlug curPlug = product.getPlug();
		JPPlug newPlug = ... // KRPlug를 JPPlug로 변경
		return newPlug;
	}
}
```

# 참고

|내용|URL|
|:---|:---|
|어댑터 패턴|https://ko.wikipedia.org/wiki/%EC%96%B4%EB%8C%91%ED%84%B0_%ED%8C%A8%ED%84%B4|