
# 파사드(Facade)

- 라이브러리에 대한, 프레임워크에 대한 또는 다른 클래스들의 복잡한 집합에 대한 단순화된 인터페이스를 제공하는 구조적 디자인 패턴
> 상점에 무언가를 주문하기 위해 전화를 걸면 받는 상담원이 일종의 파사드이다. 클라이언트는 상점 내부의 복잡한 주문, 지불, 배송에 대해 알 필요가 없이 상담원이라는 인터페이스를 통해 원하는 결과를 얻을 수 있다

# 장점
- 복잡한 시스템을 추상화하여 간단하게 사용
- 하위 인터페이스가 변경되어도 클라이언트는 변화 없음
- 클라이언트의 하위 인터페이스 의존성을 낮춘다
> 파사드 객체는 하나만 있어도 충분할 때가 있으므로 경우에 따라 싱글톤 패턴을 적용할 수도 있다
> 반대로 파사드 객체는 상속을 받아 기능을 늘려갈 수도 있다

# 단점
- 파사드 클래스가 God Object가 될 수 있다
- 파사드 클래스가 하위 인터페이스에 의존성을 가지게 된다
- 코드량 증가

# 예제
내가 비디오 프레임워크를 사용하여 다른 포맷으로 컨버팅을 하는 기능만을 필요로 한다고 할 때 클라이언트는 컨버팅 하는 방법만 알면된다. 파사드는 컨버팅하는 메서드만 제공을 하고, 상세한 구현은 파사드 내부에 구현함으로 클라이언트는 비디오 프레임워크에 대한 의존성에서 벗어날 수 있다
```java
class VideoFile
// …

class OggCompressionCodec
// …

class MPEG4CompressionCodec
// …

class CodecFactory
// …

class BitrateReader
// …

class AudioMixer
// …
// 퍼사드 클래스를 만들어 프레임워크의 복잡성을 간단한 인터페이스 뒤에 숨길 수
// 있습니다. 기능성과 단순함을 상호보완하는 것이죠.
class VideoConverter is
    method convert(filename, format):File is
        file = new VideoFile(filename)
        sourceCodec = (new CodecFactory).extract(file)
        if (format == "mp4")
            destinationCodec = new MPEG4CompressionCodec()
        else
            destinationCodec = new OggCompressionCodec()
        buffer = BitrateReader.read(filename, sourceCodec)
        result = BitrateReader.convert(buffer, destinationCodec)
        result = (new AudioMixer()).fix(result)
        return new File(result)

// 애플리케이션 클래스들은 복잡한 프레임워크에서 제공하는 수많은 클래스에 의존하지
// 않습니다. 또한 프레임워크의 전환을 결정한 경우에는 퍼사드 클래스만 다시 작성하면
// 됩니다.
class Application is
    method main() is
        convertor = new VideoConverter()
        mp4 = convertor.convert("funny-cats-video.ogg", "mp4")
        mp4.save()

```

# 참고

|내용|URL|
|:---|:---|
|퍼사드 패턴|https://ko.wikipedia.org/wiki/%ED%8D%BC%EC%82%AC%EB%93%9C_%ED%8C%A8%ED%84%B4|
|디자인 패턴|https://namu.wiki/w/%EB%94%94%EC%9E%90%EC%9D%B8%20%ED%8C%A8%ED%84%B4#s-4.5|