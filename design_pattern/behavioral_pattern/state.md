
# 상태(State)

- 객체가 특정 상태에 따라 행위를 달리하는 상황에서, 상태를 조건문으로 검사해서 행위를 달리하는 것이 아닌, 상태를 객체화 하여 상태가 행동을 할 수 있도록 위임하는 패턴
> **상태**란 객체가 가질 수 있는 어떤 조건이나 상황을 의미한다
> 켜진 상태의 TV의 경우 음량 버튼이 있고 음량을 올리거나 줄일 수 있다
> 하지만 꺼진 상태의 TV는 음량 버튼이 동작하지 않는다

# 특징
- Context : State를 이용하는 시스템, 상태 변경시 State객체에 행위 실행을 위임한다
- AbstractState : 상태를 추상화한 인터페이스
- ConcreteState : 구현된 상태
> State 클래스는 싱글톤 클래스로 구성한다, 상태는 그 객체의 현재 상태이기 때문에 대부분의 상황에서 유일하기 때문

# 장점
- 상태에 따라 다른 동작을 별도 클래스로 정의할 수 있어 상태가 증가해도 기존 코드에 영향을 미치지 않는다
- 하나의 상태 객체만 사용하여 상태 변경을 하므로 일관성 없는 상태 주입을 방지하는데 도움이 된다.

# 단점
- 상태 갯수 만큼 클래스가 증가
- 상태가 많고, 자주 변경될 경우 Context의 상태 변경 코드가 복잡해지게 될 수 있다
> 반대로 상태 변경이 거의 없거나, 변경할 상태의 수가 적다면 과한 패턴일 수도 있다

# 다른 패턴과 차이점
- 전략 : 전략 패턴은 **상속**을 대체하여 쉽게 코드를 변경할 수 있게 해주는 것에 반해 상태 패턴은 동일한 동작을 수행할 때의 조건문을 대체한다

# 예제

```java
public interface ElevatorState {
    public void pushUpButton();
    public void pushDownButton();
    public void pushStopButton();
}

public class UpState implements ElevatorState {
    private static UpState upState;

    private UpState() {}

    public static UpState getInstance() {
        if (upState == null) {
            upState = new UpState();
        }
        return upState;
    }

    @Override
    public void pushUpButton() {
        System.out.println("동작 없음");
    }

    @Override
    public void pushDownButton() {
        System.out.println("내려감");
    }

    @Override
    public void pushStopButton() {
        System.out.println("멈춤");
    }
}

public class DownState implements ElevatorState {
    private static DownState downState;

    private DownState() {}

    public static DownState getInstance() {
        if (downState == null) {
            downState = new DownState();
        }
        return downState;
    }

    @Override
    public void pushUpButton() {
        System.out.println("올라감");
    }

    @Override
    public void pushDownButton() {
        System.out.println("동작 없음");
    }

    @Override
    public void pushStopButton() {
        System.out.println("멈춤");
    }
}

public class StopState implements ElevatorState {

    private static StopState stopState;

    private StopState() {}

    public static StopState getInstance() {
        if (stopState == null) {
            stopState = new StopState();
        }
        return stopState;
    }

    @Override
    public void pushUpButton() {
        System.out.println("올라감");
    }

    @Override
    public void pushDownButton() {
        System.out.println("내려감");
    }

    @Override
    public void pushStopButton() {
        System.out.println("동작 없음");
    }
}

public class AdaptElevator {
  private ElevatorState elevatorState;

  public AdaptElevator() {
      this.elevatorState = StopState.getInstance();
  }

  public void setElevatorState(ElevatorState state) {
      this.elevatorState = state;
  }

  public void pushUpButton() {
      elevatorState.pushUpButton();
      this.setElevatorState(UpState.getInstance());
  }

  public void pushDownButton() {
      elevatorState.pushDownButton();
      this.setElevatorState(DownState.getInstance());
  }

  public void pushStopButton() {
      elevatorState.pushStopButton();
      this.setElevatorState(StopState.getInstance());
  }
}

public class Client {
    public static void main(String[] args) {
        AdaptElevator elevator = new AdaptElevator();

        elevator.pushStopButton();
        elevator.pushDownButton();
        elevator.pushStopButton();
        elevator.pushUpButton();
        elevator.pushStopButton();
        elevator.pushUpButton();
        elevator.pushUpButton();
    }
}
```

# 참고

|내용|URL|
|:---|:---|
|상태 패턴|https://ko.wikipedia.org/wiki/%EC%83%81%ED%83%9C_%ED%8C%A8%ED%84%B4|
|상태(State) 패턴 - 완벽 마스터하기|https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90#%EC%83%81%ED%83%9C_%ED%8C%A8%ED%84%B4_%EC%9E%A5%EC%A0%90|