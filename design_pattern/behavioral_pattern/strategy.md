
# 전략(Strategy)
- 객체들이 할 수 있는 행위 각각에 대해 전략 클래스를 생성하고, 유사한 행위들을 캡슐화 하는 인터페이스를 정의하여, 객체의 행위를 동적으로 바꾸고 싶은 경우 직접 행위를 수정하지 않고 전략을 바꿔주기만 함으로써 행위를 유연하게 확장하는 방법
> 여행자들이 여행을 가서 관광지를 선택하는데 수많은 고려사항이 있을 것이고 여행자마다 선택 방법이 다르고, 심지어 현재 상황(남은 시간, 돈 등)에 따라 또 달라질 수 있다. 그러한 수많은 사항들을 상속이란 구조로 구현하려면 클래스 나열하다가 끝날 것이다. 따라서 여행자에 상황에 맞는 전략을 별도로 설정할 수 있다면, 그러한 수많은 구조가 필요하지 않고 여행자마다 다른 선택 기준을 구현하여 설정하기만 하면 된다. (여행자라는 클래스를 건드리지 않았지만 모든 여행자는 다른 전략에 따라 관광지를 선택할 수 있는 것이다)

# 특징
- 전략 사용자(context) : 전략을 사용하는 프로그램의 흐름
- 전략(strategy) : 구체화된 여러 알고리즘들의 추상화
- 전략 콘크리트(Concrete strategy) : 알고리즘의 실제 구현체
- 전략 제공자(Client) : 전략 사용자에게 전략 콘크리트를 주입하는 역할
> 컨텍스트는 어떠한 기능을 실행할 때 자신에게 설정된 전략에게 맡긴다.

# 장점
- 런타임시에 실시간으로 선택할 수 있다
- 코드가 유연하고, 확장이 쉽다
- 테스트가 쉽다

# 단점
- 추가적인 클래스 및 인터페이스가 많이 필요하여 코드 복잡성이 증가될 수 있다
- 런타임시에 실시간으로 선택하는데 오버헤드 발생 가능성
- 분석 설계가 어렵다


# 예제

```java
public interface MovableStrategy {
    public void move();
}

public class RailLoadStrategy implements MovableStrategy{
    public void move(){
        System.out.println("선로를 통해 이동");
    }
}

public class LoadStrategy implements MovableStrategy{
    public void move() {
        System.out.println("도로를 통해 이동");
    }
}

public class Moving {
    private MovableStrategy movableStrategy;

    public void move(){
        movableStrategy.move();
    }

    public void setMovableStrategy(MovableStrategy movableStrategy){
        this.movableStrategy = movableStrategy;
    }
}

public class Bus extends Moving{

}

public class Train extends Moving{

}

public static void main(String args[]){
    Moving train = new Train();
    Moving bus = new Bus();

    /*
        기존의 기차와 버스의 이동 방식
        1) 기차 - 선로
        2) 버스 - 도로
    */
    train.setMovableStrategy(new RailLoadStrategy());
    bus.setMovableStrategy(new LoadStrategy());

    train.move();
    bus.move();

    /*
        선로를 따라 움직이는 버스가 개발
    */
    bus.setMovableStrategy(new RailLoadStrategy());
    bus.move();
}
```

# 참고

|내용|URL|
|:---|:---|
|전략 패턴|https://ko.wikipedia.org/wiki/%EC%A0%84%EB%9E%B5_%ED%8C%A8%ED%84%B4|
|Strategy Pattern (전략 패턴)|https://os94.tistory.com/128|