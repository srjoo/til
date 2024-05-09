
# 팩토리 메서드(Factory Method)

- 객체 생성시 어떤 클래스의 인스턴스를 만들지 서브 클래스에서 결정
- 만약 새로운 구현 클래스가 추가된다면 새로운 팩토리를 추가하면 된다

>**왜 팩토리를 쓰나?**
> - 객체는 여러 곳에서 생성될 수 있는데, 호출하는 쪽이 객체의 생성자에 직접 의존하고 있으면 나중에 변경되었을 때 수정되어야 하는 코드가 많이 발생
> - 기능 확장시 기존 코드의 변경이 없어도 된다는 점(코드를 추가하기만 하면 된다!)

# Simple Factory
-   기본적인 Simple Factory는 디자인 패턴으로 분류되지 않는다. 아래와 같이 `Switch` 와 같은 문을 사용하여 만들 경우 객체의 종류가 늘어남에 따라 계속 수정되어야 하기 때문에 OCP 원칙에 어긋나기 때문이다.
>OCP 원칙 : 개방-폐쇄 원칙(OCP, Open-Closed Principle)은 '**소프트웨어 개체(클래스, 모듈, 함수 등등)는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다**'는 프로그래밍 원칙
```java
class PetFactory {
	public Pet createPet(int petType) {
		switch (petType) {
		// 구현
		}
	}
}
```

# 예제

- 기존에 개와 고양이 객체만 지원했다고 가정
```java
class PetFactory {
	public Pet newInstance() {
		return createPet();
	}
	protected abstract Pet createPet();
}

class DogFactory extends PetFactory {
// createPet 구현
}

class CatFactory extends PetFactory {
// createPet 구현
}

void main() {
	Pet pet;
	if(type == "Dog") {
		pet = new DogFactory().newInstance();
	} else if(type == "Cat") {
		pet = new CatFactory().newInstance();
	}
}
```
- 새로운 동물로 햄스터가 추가된다. 아래의 코드와 같이 기존 코드를 전혀 수정하지 않고 신규 코드를 추가하는 것만으로 모든 것이 해결된다
```java
class HamsterFactory extends PetFactory {
// createPet 구현
}

void main() {
// 생략
	else if(type == "Hamster") {
		pet = new HamsterFactory().newInstance();
	}
}
```
 

# 참고

|내용|URL|
|:---|:---|
|Factory 패턴 (2/3) - Factory Method (팩토리 메서드) 패턴|https://bcp0109.tistory.com/367|
[Factory 패턴 (1/3) - Simple Factory|https://bcp0109.tistory.com/366|