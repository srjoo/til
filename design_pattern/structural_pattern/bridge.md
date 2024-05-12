# 브리지(Bridge)

-  대규모 클래스 혹은 **밀접하게 연관된** 클래스 집합을 서로 독립적으로 개발 가능하도록 두개의 개별 계층으로 분할하는 패턴
> 일반적으로 기능과 구현 계층으로 구분할 수 있다
> 예를 들면 Door 클래스로 문의 기능을 구현하고, RawDoor 이라는 클래스로 실제 문의 움직임을 구현한다 라고 할 때, 크게 보면 문에는  문을 연다, 문을 닫는다 라는 기능이 있을 것이고, 구현 계층에는 문을 어떻게 열지, 어떻게 닫을지 구현해야 될 것이다
> 어떤 문은 자동 잠금장치가 있을 수도 있고, 어떤 문은 아무런 장치도 없을 것이고 이러한 것들은 문 마다 다를 것이다
> 하지만 Door 클래스 입장에서는 "문을 연다", "문을 닫는다" 라는 개념만 생각하면 되는 것이다

# 장점
- 추상화된 인터페이스(기능)와 구현된 클래스를 분리하여 유연한 확장이 가능하다
- 코드의 재사용성을 높여준다
- 코드의 가독성을 높여준다

# 단점
-   코드의 복잡성이 증가한다
-   추상화된 인터페이스와 구현된 클래스를 분리하는 것이 코드의 중복을 일으킬 수 있다
-   브리지 패턴을 적용하면 런타임에 객체가 생성되므로 성능 저하가 발생할 수 있다

# 예제
내 집에는 2개의 문이 있고 1개는 열쇠 문, 1개는 디지털 도어락 문이라고 할 때 그 문을 열고 닫는 것을 구현해보자
이러한 코드를 구현할 때 브리지 패턴을 써서 구현한다면 문을 연다, 문을 닫는다라는 행동을 할 때 문마다 다른 여는 방법에 대해서 고민할 필요가 없을 것이다
```java
interface Door {
	void open();
	void close();
}

interface RawDoor {
	void rawOpen();
	void rawClose();
}

class KeyRawDoor implements RawDoor {
	public void rawOpen() {
		System.out.println("열쇠를 돌려 자물쇠를 푼다");
		System.out.println("문 열림");
	}
	
	public void rawClose() {
		System.out.println("문 닫힘");
		System.out.println("열쇠를 돌려 자물쇠를 건다");
	}
}

class DigitalDoorLockRawDoor implements RawDoor {
	public void rawOpen() {
		System.out.println("도어락에 비밀번호를 입력한다");
		System.out.println("도어락이 풀린다");
		System.out.println("문 열림");
	}
	
	public void rawClose() {
		System.out.println("문 닫힘");
		System.out.println("도어락이 잠긴다");
	}
}

class HouseDoor implements Door {
	private RawDoor rawDoor;
	public HouseDoor(RawDoor rawDoor) {
		this.rawDoor = rawDoor;
	}

	public void open() {
		rawDoor.rawOpen();
	}

	public void close() {
		rawDoor.rawClose();
	}
}

void main() {
	HouseDoor[] myHouseDoor = new HouseDoor[2];
	myHouseDoor[0] = new HouseDoor(new KeyRawDoor());
	myHouseDoor[1] = new HouseDoor(new DigitalDoorLockRawDoor());
	myHouseDoor[0].open();
	myHouseDoor[0].close();
	myHouseDoor[1].open();
	myHouseDoor[1].close();
}
```

# 참고

|내용|URL|
|:---|:---|
|브리지 패턴|https://ko.wikipedia.org/wiki/%EB%B8%8C%EB%A6%AC%EC%A7%80_%ED%8C%A8%ED%84%B4|
|Bridge 패턴 (기능계층과 구현계층 분리하기)|https://m.blog.naver.com/tradlinx0522/220928963011|