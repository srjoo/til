
# 컴포지트(Composite)

-  객체들의 관계를 트리구조로 구성하고 단일 객체나 복합 객체나 동일하게 작업할 수 있게 하는 패턴
> 트리 구조로 표현할 수 있는 경우 컴포지트가 의미가 있다
> 대표적으로 안드로이드의 경우 View, ViewGroup 이 이러한 관계를 기반으로 만들어져 있다

# 기본 구조
- 트리형이기 때문에 최상위 노드(인터페이스), 중간 노드(컨테이너), 리프로 구분할 수 있다
- 최상위 노드는 중간 노드와 리프의 공통적인 작업의 인터페이스를 정의한다
- 리프는 작업에 대해 더이상 전달할 대상이 없기 때문에 실질적인 작업을 수행하게 된다
- 중간 노드는 자식의 구체적인 클래스를 알지 못하며, 인터페이스를 통해 작업을 위임한 다음 결과를 상위 노드에게 반환한다

# 장점
- 복잡한 트리 구조를 간편하게 작업할 수 있다
- 기존 코드를 수정하지 않아도 확장이 자유롭다

# 단점
-   기능이 너무 많이 다른 경우 공통 인터페이스 정의가 어렵다

# 예제
내가 군대의 장군이라고 할 때 부대에 명령을 내릴려면 어떻게 내리는가? 모든 소규모 부대에 직접 전달하지는 않는다. 상급 부대에 명령을 내리고, 상급 부대가 나를 **대리**하여 명령을 전달하게 되어 있다. 컴포지트 패턴을 이용하여 이것을 표현하면 아래와 같다.
```java
interface Army {
	void doOrder(String order);
}

interface ArmyGroup extends Army {
	void add(Army child);
}

class ArmyDivision implements ArmyGroup {
	String unitName;

	public ArmyDivision(String unitName) {
		this.unitName = unitName;
	}
	
	public void doOrder(String order) {
		System.out.println(unitName + "이/가 명령을 전달합니다");
		// 자식들의 doOrder 호출
	}
	
	public void add(Army child) {
		// 내부의 리스트에 child 추가
	}
}

class ArmyUnit implements Army {
	String unitName;

	public ArmyUnit(String unitName) {
		this.unitName = unitName;
	}
	
	public void doOrder(String order) {
		System.out.println(unitName + "이/가 명령을 수행합니다 : " + order);
	}
}

void main() {
	ArmyDivision div0 = new ArmyDivision("국방부");
	ArmyDivision div1 = new ArmyDivision("1사단");
	ArmyDivision div2 = new ArmyDivision("2사단");
	ArmyUnit unit11 = new ArmyUnit("1연대 1대대");
	ArmyUnit unit12 = new ArmyUnit("1연대 2대대");
	ArmyUnit unit21 = new ArmyUnit("2연대 1대대");
	ArmyUnit unit22 = new ArmyUnit("2연대 2대대");
	div0.add(div1);
	div0.add(div2);
	div1.add(unit11);
	div1.add(unit12);
	div2.add(unit21);
	div2.add(unit22);
	Army army = div0;
	army.doOrder("목표 지점으로 이동하라");
}
```

# 참고

|내용|URL|
|:---|:---|
|컴포지트 패턴|https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%8F%AC%EC%A7%80%ED%8A%B8_%ED%8C%A8%ED%84%B4|
|컴포지트 패턴(Composite Pattern)|https://mygumi.tistory.com/343|