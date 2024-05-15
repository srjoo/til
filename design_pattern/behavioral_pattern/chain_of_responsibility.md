
# 책임 연쇄(Chain of Responsibility)

- 핸들러들의 체인​(사슬)​을 따라 요청을 전달할 수 있게 해주는 행동 디자인 패턴
> if-else 조건문, 혹은 중앙집권화된 로직을 핸들러 객체로 분리하여 책임을 분산시키므로서 코드 퀄리티를 높인다

# 장점
- 클라이언트는 체인 집합 내부의 구조를 알 필요가 없다
- 요청의 발신자와 수신자를 분리할 수 있어 결합도를 낮춘다
- 집합 내의 처리 추가/삭제/변경이 쉽다

# 단점
- 내부에서 사이클이 발생할 수 있다
- 디버깅, 테스트 어려움

# 예제

```java
public class Request {
    private String type;
    private String message;

    public Request(String type, String message) {
        this.type = type;
        this.message = message;
    }

    public String getType() {
        return type;
    }

    public String getMessage() {
        return message;
    }
}

public interface Handler {
    void setNext(Handler handler);
    void handleRequest(Request request);
}

public class FirstHandler implements Handler {
    private Handler nextHandler;

    @Override
    public void setNext(Handler handler) {
        nextHandler = handler;
    }

    @Override
    public void handleRequest(Request request) {
        if (request.getType().equals("type1")) {
            System.out.println("Type 1 request handled by FirstHandler: " + request.getMessage());
        } else if (nextHandler != null) {
            // 다음 handler에 처리를 전가
            nextHandler.handleRequest(request);
        }
    }
}

public class SecondHandler implements Handler {
    private Handler nextHandler;

    @Override
    public void setNext(Handler handler) {
        nextHandler = handler;
    }

    @Override
    public void handleRequest(Request request) {
        if (request.getType().equals("type2")) {
            System.out.println("Type 2 request handled by SecondHandler: " + request.getMessage());
        } else if (nextHandler != null) {
            // 다음 handler에 처리를 전가
            nextHandler.handleRequest(request);
        }
    }
}

public class ThirdHandler implements Handler {
    private Handler nextHandler;

    @Override
    public void setNext(Handler handler) {
        nextHandler = handler;
    }

    @Override
    public void handleRequest(Request request) {
    	if (request.getType().equals("type3")) {
            System.out.println("Type 3 request handled by ThirdHandler: " + request.getMessage());
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        } else {
            // 다음 handler가 없기때문에 처리를 전가하지않음
            System.out.println("Request not handled.");
        }
    }
}

public static void main(String[] args) {
    Handler firstHandler = new FirstHandler();
    Handler secondHandler = new SecondHandler();
    Handler thirdHandler = new ThirdHandler();

    firstHandler.setNext(secondHandler);
    secondHandler.setNext(thirdHandler);

    Request request1 = new Request("type1", "Request 1");
    Request request2 = new Request("type2", "Request 2");
    Request request3 = new Request("type3", "Request 3");
    Request request4 = new Request("type4", "Request 4");

    firstHandler.handleRequest(request1);
    firstHandler.handleRequest(request2);
    firstHandler.handleRequest(request3);
    firstHandler.handleRequest(request4);
}
```

# 참고

|내용|URL|
|:---|:---|
|책임 연쇄 패턴|https://refactoring.guru/ko/design-patterns/chain-of-responsibility|