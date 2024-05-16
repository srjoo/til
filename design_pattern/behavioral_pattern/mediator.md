
# 중재자(Meditator)

- 객체 간의 직접 통신을 제한하고 중재자 객체를 통해서만 통신하는 패턴

# 특징
- Mediator : Colleague 객체간의 커뮤니케이션을 위한 인터페이스 정의
- Colleague : Mediator를 통해 다른 Colleague와 커뮤니케이션을 위한 인터페이스 정의
- ConcreteMediator : Mediator 구현체로 Colleague들간의 상호 커뮤니케이션을 위해 Colleague들을 가지고 있으며 커뮤니케이션을 조정함
- ConcreteColleague : Colleague 인터페이스 구현체

# 장점
- 효율적인 자원 관리가 가능
- 객체간의 통신을 위해 서로 직접 참조할 필요가 없다

# 단점
- 객체간 중개 통신 로직이 복잡해지거나 자주 바뀌는 경우 유지보수 어려움

# 다른 패턴과 차이점
- 옵저버 : 옵저버는 1개의 발행자에 N개의 구독자를 가진다, 반면 중재자는 M개의 발행자와 N개의 구독자를 사이에서 1개의 중개자를 통해 통신한다

# 예제

```kotlin
interface Mediator {
    fun addUser(user: User)
    fun sendMessage(user: User, msg: String)
}

interface User {
    val mediator: Mediator
    fun receive(msg: String)
    fun send(msg: String)
}

class ManUser(
    override val mediator: Mediator
) : User {
    override fun receive(msg: String) {
        println("Man got msg: $msg")
    }

    override fun send(msg: String) {
        mediator.sendMessage(this, msg)
    }

}

class WomanUser(
    override val mediator: Mediator
) : User {
    override fun receive(msg: String) {
        println("Woman got msg: $msg")
    }

    override fun send(msg: String) {
        mediator.sendMessage(this, msg)
    }
}

class UserMessageMediator : Mediator {

    private val userList = arrayListOf<User>()

    override fun addUser(user: User) {
        userList.add(user)
    }

    override fun sendMessage(user: User, msg: String) {
        userList.forEach {
            if (it != user) it.receive(msg)
        }
    }
}

fun main() {
    val mediator = UserMessageMediator()
    val man = ManUser(mediator)
    val woman = WomanUser(mediator)

    mediator.addUser(man)
    mediator.addUser(woman)

    man.send("Hi, how are you?")
    woman.send("I am good!")
}
// Woman got msg: Hi, how are you?
// Man got msg: I am good!
```

# 참고

|내용|URL|
|:---|:---|
|중재자 패턴|https://ko.wikipedia.org/wiki/%EC%A4%91%EC%9E%AC%EC%9E%90_%ED%8C%A8%ED%84%B4|
|[Design Pattern] 중재자 패턴(Mediator Pattern)|https://velog.io/@wlsrhkd4023/Design-Pattern-%EC%A4%91%EC%9E%AC%EC%9E%90-%ED%8C%A8%ED%84%B4Mediator-Pattern|