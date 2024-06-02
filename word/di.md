# DI(Dependency Injection)
- 의존성 주입

# 의존성(Dependency)
어떤 "서비스"를 호출하려는 그 "클라이언트"는 그 "서비스"가 어떻게 구성되었는지 알지 못해야 한다.
"클라이언트"는 대신 서비스 제공에 대한 책임을 외부 코드(주입자)로부터 위임한다.
"클라이언트"는 주입자 코드를 호출할 수 없다.
그 다음, 주입자는 이미 존재하거나 주입자에 의해 구성되었을 서비스를 클라이언트로 주입(전달)한다.
그러고 나서 클라이언트는 서비스를 이용한다.
이는 클라이언트가 주입자와 서비스 구성 방식 또는 사용중인 실제 서비스에 대해 알 필요가 없음을 의미한다.
"클라이언트" : 서비스의 사용 방식을 정의하고 있는 서비스의 고유한 인터페이스에 대해서만 알면 된다.
구성의 책임으로부터 사용의 책임을 구분한다.

# 의존성 있는 코드
- 커피를 마시는 프로그래머가 있다면, 아래 코드의 경우 어떤 커피를 마실지 미리 정해놔야한다
- 만약 커피의 종류가 늘어나면 아래 코드로는 ProgrammerB 등 클래스를 늘리거나, 코드를 수정하는 수 밖에 없다
```java
public class Coffee {...}

public class Programmer {
	private Coffee coffee = new Coffee();

	public startProgramming(){
		this.coffee.drink()
	}
}
```

# 의존성 제거 코드
- 위의 코드에서 프로그래머가 외부로부터 커피 객체를 받음으로써(주입) 더이상 프로그래머 객체는 커피의 종류와 상관없이 사용(재사용) 가능하다
```java
public class Coffee {...}
public class Cappuccino extends Coffee {...}
public class Americano extends Coffee {...}

public class Programmer {
	private Coffee coffee;

	public Programmer(Coffee coffee){
		this.coffee = coffee
	}

	public startProgramming(){
		this.coffee.drink()
	}
}
```

# 장점
- Unit Test가 용이해진다. ⇒ 의존성이 낮아지니깐
- 코드의 재활용성이 높아진다. ⇒ 하나의 객체에 의존하지 않으니깐
- 객체 간의 의존성(종속성)을 줄이거나 없앨 수 있다.
- 객체 간의 결합도를 낮추면서 유연한 코드를 작성할 수 있다.

# 방법
1. 생성자 주입(Constructor Injection)
- 생성자에서 주입

2. 수정자 주입(Setter Injection)
- setter를 이용하여 주입

3. 필드 주입(Field Injection)
- 필드에 직접 접근하여 주입

> 생성자 주입이 제일 권장됨
> 1. 객체의 불변성 확보
> 2. 테스트 코드의 작성
> 3. final 키워드 작성 및 Lombok의 결합
> 위 3가지 방법중 생성자 주입만이 객체의 생성과 "동시에" 의존성 주입이 이루어진다. 그래서 final변수를 할당 할 수 있다. 나머지 방법은 객체의 생성 ⇒ 의존성 주입 함수 호출로 이루어지기에 final변수를 할당 할 수 없다.
> 4. 순환 참조 에러 방지