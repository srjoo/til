
# 싱글톤 패턴(Singleton Pattern)

- 객체의 인스턴스가 1개만 생성되는 패턴
> 싱글톤 패턴을 사용하는 것이 좋은 설계가 되는 경우
> - 속성이 없거나, 읽기 전용인 경우
> - 인스턴스 메소드가 하나 이상 있는 경우
> 
> **하지만 위의 조건을 만족하는 경우는 별로 없다**

# 장점
- 메모리 이점(1개의 객체만 있기 때문)
- 쉬운 데이터 공유

# 단점
- 구현시 방대한 코드가 필요할 수 있다
- 다중 스레드의 동시성 문제
- 테스트가 어려움 (어플리케이션 전역에서 상태를 공유하기 때문에)
- 싱글톤 클래스 의존도가 높아진다(사이드 이펙트 확률 높아짐)
- 싱글톤 클래스의 서브클래스를 만들기 어렵다

# 오남용?
- 단순 데이터 저장용 클래스를 싱글톤으로 만드는 경우가 많다

# 사용 예
- 추상 팩토리 패턴의 콘크리트 팩토리 클래스
- 빌더 패턴의 내부 빌더 클래스
- 프로토타입 패턴의 콘크리트 프로토타입 클래스
- 파사드 패턴에서 파사드 클래스가 속성을 갖지 않는 경우

# 구현
- Eager initialization(사전 초기화)
> 클래스 로딩시 인스턴스 생성
> - 멀티스레드 환경에서 이중 객체 생성 문제가 없지만 인스턴스 호출을 하지 않아도 초기화하기 때문에 메모리 효율, 연산 효율은 낮다
```java
class Singleton {

    static final Singleton instance = new Singleton();

    private Singleton() {
        //초기화
    }

    public Singleton getInstance() {
        return instance;
    }

}
```
- Lazy initialization(사후 초기화)
> 사용하는 시점에 인스턴스 생성
> - 멀티스레드 환경에서 이중 객체 생성 문제가 있을 수있지만 메모리 효율, 연산 효율이 높다
```java
class Singleton {

    static final Singleton instance = null;

    private Singleton() {
        //초기화
    }

    public Singleton getInstance() {
	    if(instance == null) {
		    instance = new Singleton()
	    }
        return instance;
    }

}
```

# 참고

|내용|URL|
|:---|:---|
|싱글톤 패턴|https://namu.wiki/w/%EB%94%94%EC%9E%90%EC%9D%B8%20%ED%8C%A8%ED%84%B4#s-3.5|