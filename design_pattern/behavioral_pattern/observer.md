
# 옵저버(Observer)

- 여러 객체에 자신이 관찰 중인 객체에 발생하는 모든 이벤트에 대하여 알리는 구독 메커니즘을 정의하는 패턴
- 발행/구독 모델으로도 알려져있다
> 어떠한 이벤트 혹은 값의 변경을 여러 객체들이 참조하면서 확인하는 것보다 **이벤트가 발생했을 때** 알려주는 것이 퍼포먼스, 코드적으로 훨씬 낫다

# 특징
- Subject : 관찰의 주체로써 관찰자를 관리한다(등록/삭제/알림 기능)
- Observer : 관찰자로서 알림 기능을 가지고 있다
- ConcreteObserver : 구현된 관찰자

# 장점
- 실시간으로 객체의 변경사항을 전파할 수 있다
- 코드 변경 없이 새 구독자 클래스를 도입할 수 있다(OCP 준수)
- 런타임 시점에 구독 관계를 맺을 수 있다
- 느슨한 결함(Loose Coupling)으로 객체간의 의존성을 제거할 수 있다

# 단점
- 알림 순서를 제어할 수 없다
> 코드로 구현할 수 있지만 복잡성과 결합성이 높아지므로 추천되지 않는다
- 과도하게 사용할 경우 상태 관리가 어려울 수 있다
> 재귀 호출의 가능성에 유의해야한다
- 코드 복잡도 증가
- 메모리 누수 발생 가능성
> 옵서버 객체를 등록 이후 해지하는 것이 필요

# 예제

```java
public interface Publisher {
	public void add(Observer observer);
    public void delete(Observer observer);
    public void notifyObserver();
}

public interface Observer {
	public void update(String title, String news);
}

public class NewsMachine implements Publisher {
	private ArrayList<Observer> observers;
    private String title;
    private String news;
    
    public NewsMachine() {
    	observers = new ArrayList<>();
    }
    
    @Override
    public void add(Observer observer) {
    	observers.add(observer);
    }
    
    @Override
    public void delete(Observer observer) {
    	int index = observers.indexOf(observer);
    	observers.remove(index);
    }
    
    @Override
    public void notifyObserver() {
    	for (Observer Observer : observers) {
        	observer.update(title, news);
        }
    }
    
    public void setNewsInfo(String title, String news) {
    	this.title = title;
        this.news = news;
        notifyObserver();
    }
    
    public String getTitle() {
    	return title;
    }
    
    public String getNews() {
    	return news;
    }
}

public AnnualSubscriber implement Observer {
	private String newsString;
    private Publisher publisher;
    
    public AnnualSubscriber(Publisher publisher) {
    	this.publisher = publisher;
        publisher.add(this);
    }
    
    @Override
    public void update(String title, String news) {
    	this.newsString = title + " \n--------\n " + news;
        display();
    }
    
    private void display() {
    	System.out.println("\n\n오늘의 뉴스\n============================\n\n" + newsString);
    }
}

public class EventSubscriber implements Observer {
	private String newsString;
    private Publisher publisher;
    
    public EventSubscriber(Publisher publisher) {
    	this.publisher = publisher;
        publisher.add(this);
    }
    
    @Override
    public void update(String title, String news) {
    	newsString = title + "\n------------------------------------\n" + news;
        display();
    }
    
    private void display() {
    	System.out.println("\n\n=== 이벤트 유저 ===");
        System.out.println("\n\n" + newsString);
    }
}

public class MainClass {
	public static void main(String[] args) {
    	NewsMachine newsMachine = new NewsMachine();
        AnnualSubscriber annualSubscriber = new AnnualSubscriber(newsMachine);
        EventSubscriber eventSubscriber = new EventSubscriber(newsMachine);
        
        newsMachine.setNewsInfo("오늘 한파", "전국 영하 18도 입니다.");
        newsMachine.setNewsInfo("벛꽃 축제합니다", "다같이 벚꽃보러~");
    }
}
```

# 참고

|내용|URL|
|:---|:---|
|옵서버 패턴|https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4|
|옵저버 패턴(Observer Pattern)|https://velog.io/@octo__/%EC%98%B5%EC%A0%80%EB%B2%84-%ED%8C%A8%ED%84%B4Observer-Pattern|