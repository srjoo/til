
# 프록시(Proxy)

-  프록시는 대리인이라는 뜻으로, 무엇인가를 대신 처리해주는 패턴
- 어떤 객체를 사용하고자 할 때, 객체를 직접 참조하는 것이 아니라 해당 객체를 대행(대리, proxy)하는 객체를 통해 대상 객체에 접근하는 방식을 사용하면 해당 객체가 메모리에 존재하지 않아도 기본적인 정보를 참조하거나 설정할 수 있고,  
실제 객체의 기능이 반드시 필요한 시점까지 객체의 생성을 미룰 수 있다.

# 장점
- 기존 객체와 클라이언트 사이에 있기 때문에 무분별한 접근 방지 역할을 할 수 있
- 기존 코드를 수정하지 않고 기능을 추가할 수 있다
- 유연한 코드를 만들 수 있다(전처리, 캐싱 등도 가능하다)

# 단점
- 성능이 저하될 수 있다
- 코드가 복잡해진다

# 예제

```java
// 원격 서비스의 인터페이스.
interface ThirdPartyYouTubeLib is
    method listVideos()
    method getVideoInfo(id)
    method downloadVideo(id)

// 서비스 연결자의 구상 구현. 이 클래스의 메서드들은 유튜브에서 정보를 요청할 수
// 있습니다. 해당 요청의 속도는 사용자와 유튜브의 인터넷 연결 속도에 따라 다를
// 것입니다. 앱이 많은 요청을 동시에 처리하면 속도가 느려질 것입니다. 이는
// 요청들이 모두 같은 정보를 요청하더라도 마찬가지입니다.
class ThirdPartyYouTubeClass implements ThirdPartyYouTubeLib is
    method listVideos() is
        // 유튜브에 API 요청을 보냅니다.

    method getVideoInfo(id) is
        // 어떤 비디오에 대한 메타데이터를 가져옵니다.

    method downloadVideo(id) is
        // 유튜브에서 동영상 파일을 다운로드합니다.

// 일부 대역폭을 절약하기 위해 요청 결과를 캐시하고 일정 기간 보관할 수 있습니다.
// 그러나 이러한 코드를 서비스 클래스에 직접 넣는 것은 불가능할 수 있습니다. 예를
// 들어, 타사 라이브러리의 일부로 제공되었거나 `final`로 정의된 경우에는 말이죠.
// 서비스 클래스와 같은 인터페이스를 구현하는 새 프록시 클래스에 캐싱 코드를 넣는
// 이유가 바로 그 때문입니다. 이 클래스는 실제 요청을 보내야 하는 경우에만 서비스
// 객체에 위임합니다.
class CachedYouTubeClass implements ThirdPartyYouTubeLib is
    private field service: ThirdPartyYouTubeLib
    private field listCache, videoCache
    field needReset

    constructor CachedYouTubeClass(service: ThirdPartyYouTubeLib) is
        this.service = service

    method listVideos() is
        if (listCache == null || needReset)
            listCache = service.listVideos()
        return listCache

    method getVideoInfo(id) is
        if (videoCache == null || needReset)
            videoCache = service.getVideoInfo(id)
        return videoCache

    method downloadVideo(id) is
        if (!downloadExists(id) || needReset)
            service.downloadVideo(id)

// 서비스 객체와 직접 작업하던 그래픽 사용자 인터페이스 클래스는 서비스 객체와
// 인터페이스를 통해 작업하는 한 변경되지 않습니다. 둘 다 같은 인터페이스를
// 구현하므로 실제 서비스 객체 대신 프록시 객체를 안전하게 전달할 수 있습니다.
class YouTubeManager is
    protected field service: ThirdPartyYouTubeLib

    constructor YouTubeManager(service: ThirdPartyYouTubeLib) is
        this.service = service

    method renderVideoPage(id) is
        info = service.getVideoInfo(id)
        // 비디오 페이지를 렌더링하세요.

    method renderListPanel() is
        list = service.listVideos()
        // 비디오 섬네일 리스트를 렌더링하세요.

    method reactOnUserInput() is
        renderVideoPage()
        renderListPanel()

// 앱은 언제든지 프록시를 설정할 수 있습니다.
class Application is
    method init() is
        aYouTubeService = new ThirdPartyYouTubeClass()
        aYouTubeProxy = new CachedYouTubeClass(aYouTubeService)
        manager = new YouTubeManager(aYouTubeProxy)
        manager.reactOnUserInput()
```

# 참고

|내용|URL|
|:---|:---|
|프록시 패턴|https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9D%EC%8B%9C_%ED%8C%A8%ED%84%B4|
|프록시 패턴|https://refactoring.guru/ko/design-patterns/proxy|