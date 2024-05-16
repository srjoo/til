
# 메멘토(Memento)

- 객체의 상태 정보를 가지는 클래스를 따로 생성하여, 객체의 상태를 저장하거나 이전 상태로 복원할 수 있게 해주는 패턴
> 대부분의 객체들은 중요한 데이터는 캡슐화 되어있어서 원하는 데이터의 복사가 어렵다
> 만약 모든 필드가 열려있다고 해도 해당 객체들이 수정된다면 복사 로직 또한 수정되어야 하는 문제점이 생긴다
> 이러한 문제점들을 해당 상태의 실제 소유자인 originator에게 위임함으로써 해결한다

# 특징
- Originator : 현재 내부 상태를 저장하는 Memento 객체를 생성하고 돌려주는 기능, Memento 객체로 전달된 상태를 복구하는 기능 구현
- Caretaker : Originator를 참조하여 상태를 저장하고 복구한다
- Memento : 저장된 상태

# 장점
- 저장된 상태를 다른 별도의 객체에 보관하기 때문에 안전
- 원본 객체의 데이터를 계속해서 캡슐화된 상태로 유지할 수 있다
- 복구 기능을 구현하기 쉽다

# 단점
- 이전 상태의 객체를 저장하기 위한 Originator가 클 경우 많은 메모리가 필요
- 상태를 저장하고 복구하는 데 시간이 걸릴 수 있다


# 예제

```java
public class Originator {
    private String state;

    public void setState(String state){
        this.state = state;
    }

    public String getState(){
        return state;
    }

    public Memento saveStateToMemento(){
        return new Memento(state);
    }

    public void getStateFromMemento(Memento memento){
        state = memento.getState();
    }
}

public class Memento {
    private String state;

    public Memento(String state){
        this.state = state;
    }

    public String getState(){
        return state;
    }
}

public class CareTaker {
    private List<Memento> mementoList = new ArrayList<Memento>();

    public void add(Memento state){
        mementoList.add(state);
    }

    public Memento get(int index){
        return mementoList.get(index);
    }
}

 public static void main(String[] args) {
        Originator originator = new Originator();
        CareTaker careTaker = new CareTaker();
        originator.setState("State #1");
        originator.setState("State #2");
        careTaker.add(originator.saveStateToMemento());
        originator.setState("State #3");
        careTaker.add(originator.saveStateToMemento());
        originator.setState("State #4");

        System.out.println("Current State: " + originator.getState());
        originator.getStateFromMemento(careTaker.get(0));
        System.out.println("First saved State: " + originator.getState());
        originator.getStateFromMemento(careTaker.get(1));
        System.out.println("Second saved State: " + originator.getState());
    }
```

# 참고

|내용|URL|
|:---|:---|
|메멘토 패턴|https://ko.wikipedia.org/wiki/%EB%A9%94%EB%A9%98%ED%86%A0_%ED%8C%A8%ED%84%B4|
|[Design Pattern] 메멘토 패턴 (Memento Pattern)|https://velog.io/@cham/Design-Pattern-%EB%A9%94%EB%A9%98%ED%86%A0-%ED%8C%A8%ED%84%B4-Memento-Pattern|