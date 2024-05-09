
# 추상 팩토리 패턴(Abstract Factory Pattern)

- 서로 연관이 있는 여러 객체를 모두 묶어서 팩토리로 만들고 이 팩토리를 조건에 따라 생성하도 팩토리를 만드 패턴
- 팩토리 패턴을 한번 더 캡슐화한 방식

# 용어
-   AbstractFactory: 개념적 제품에 대한 객체를 생성하는 연산으로 인터페이스 정의, 팩토리 클래스의 공통 인터페이스
```java
interface MovieAbstractFactory {
	Movie getMovie();
}
```
-   ConcreteFactory: 구체적인 제품에 대한 객체를 생성하는 연산 구현, 구체적인 팩토리 클래스
```java
class RealMovie1Factory implements MovieAbstractFactory {
	@Override
	public Movie getMovie() {
		return new RealMoview1();
	}
}
```
-   AbstractProduct: 개념적 제품 객체에 대한 인터페이스 정의, 제품의 공통 인터페이스
```java
interface Movie {
	void getCast();
	void getDirector();
}
```
-   ConcreteProduct: 구체적으로 팩토리가 생성할 객체를 정의하고, AbstractProduct가 정의하는 인터페이스 구현, 구체적인 제품
```java
class RealMovie1 implements Movie {
	@Override
	void getCast() {/*구현*/}
	@Override
	void getDirector() {/*구현*/}
}
```


# 팩토리 메서드와의 차이점

- 팩토리 메서드는 직접적으로 `if-else` 등의 조건식을 사용하여 서브 클래스를 생성하고 리턴하지만 추상 팩토리 패턴에서는 다른 팩토리의 메서드를 받는다
 
|      |팩토리 메서드|추상화 팩토리|
|:---:|:---:|:---:|
|생성 범위|한 개의 메서드에서 여러 객체를 만든다|여러 개의 관련 객체를 하나의 팩토리로 만든다|
|생성되는 객체의 종류|인자에 따라 생성될 객체가 결정됨|인자에 따라 사용할 팩토리가 결정됨|
|의존관계|클라이언트와 ConcreteProduct의 의존성 적음, ConcreteFactory와의 의존성 높음|클라이언트와 ConcreteProduct의 의존성 적음, ConcreteFactory와의 의존성 적음|
|포커스|메서드 레벨|클래스 레벨|
|상속, 구성|상속|구성|

# 간단한 예시
- 고객이 나(AbstractFactory)에 소프트웨어 제작을 명세서와 함께 맡긴다
- 나는 실제 소프트웨어 제작법을 모르고, 다른 회사(ConcreteFactory)에 맡기는 방법을 안다
- 고객에게서 받은 명세서를 토대로 다른 회사에 소프트웨어를 발주하여 받는다(ConcreteProduct). 발주는 내가 하므로 고객은 소프트웨어를 발주하는 방법을 알 필요가 없다(ConcreteFactory에 대한 의존성 없음)
- 고객은 받은 제품이 어떻게 구성되어 있는지 알 필요 없이(AbstractProduct) 원하는 기능을 사용한다.

# 참고

|내용|URL|
|:---|:---|
|추상 팩토리 패턴 (Abstract Factory Pattern)|https://johngrib.github.io/wiki/pattern/abstract-factory/|