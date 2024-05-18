
# 방문자(Visitor)

- 방문자와 방문 공간을 분리하여, 방문 공간이 방문자를 맞이할 때, 이후에 대한 행동을 방문자에게 위임하는 패턴

# 특징
- Visitor : 방문자 클래스의 인터페이스
- Element : 방문 공간 클래스의 인터페이스, accept(Vistor) 를 공용 인터페이스로 쓴다, 내부적으로 Vistor.visit(this) 를 호출한다.
- ConcreteVisitor : 구현된 방문자
- ConcreteElement : 구현된 요소

# 장점
- 작업 대상(방문 공간) 과 작업 항목(방문 공간을 가지고 하는 일)을 분리시킨다.
- 데이터와 알고리즘이 분리되어, 데이터의 독립성을 높여준다.
- 작업 대상의 입장에서는 인터페이스를 통일시켜, 사용자에게 동일한 인터페이스를 제공한다
- 생각지 못했던 연산을 최소한의 변경(accept 구현)만으로 쉽게 구현 가능하다
- 방문자는 요소들을 방문하면서 상태를 축적할 수 있다
> 다른 패턴들과 가장 다른 점들이다, 파일 입출력, 요소들의 전체 속성 종합 등 여러가지를 할 수 있다

# 단점
- 새로운 작업 대상(방문 공간)이 추가될 때마다 작업 주체(방문자) 도 이에 대한 로직을 추가해야 한다.
> 두 객체 (방문자와 방문 공간)의 결합도가 높아진다


# 예제

```java
/ 요소 인터페이스는 기초 방문자 인터페이스를 인수로 받는 `accept` 메서드를
// 선언합니다.
interface Shape is
    method move(x, y)
    method draw()
    method accept(v: Visitor)

// 각 구상 요소 클래스는 요소의 클래스에 해당하는 비지터의 메서드를 호출하는
// 방식으로 `accept` 메서드를 구현해야 합니다.
class Dot implements Shape is
    // …

    // 참고로 우리는 현재 클래스 이름과 일치하는 `visitDot`를 호출하고
    // 있습니다. 그래야 비지터가 함께 작업하는 요소의 클래스를 알 수 있습니다.
    method accept(v: Visitor) is
        v.visitDot(this)

class Circle implements Shape is
    // …
    method accept(v: Visitor) is
        v.visitCircle(this)

class Rectangle implements Shape is
    // …
    method accept(v: Visitor) is
        v.visitRectangle(this)

class CompoundShape implements Shape is
    // …
    method accept(v: Visitor) is
        v.visitCompoundShape(this)


// 비지터 인터페이스는 요소 클래스들에 해당하는 방문 메서드들의 집합을 선언합니다.
// 방문 메서드의 시그니처를 통해 비지터는 처리 중인 요소의 정확한 클래스를 식별할
// 수 있습니다.
interface Visitor is
    method visitDot(d: Dot)
    method visitCircle(c: Circle)
    method visitRectangle(r: Rectangle)
    method visitCompoundShape(cs: CompoundShape)

// 구상 비지터는 모든 구상 요소 클래스와 작동할 수 있는 같은 알고리즘의 여러 버전을
// 구현합니다.
//
// 비지터 패턴은 복합체 트리와 같은 복잡한 객체 구조와 함께 사용할 때 가장 큰
// 이득을 볼 수 있습니다. 그러면 비지터의 메서드들을 구조의 다양한 객체 위에서
// 실행하는 동안 알고리즘의 어떤 중간 상태를 저장하는 것이 도움이 될 수 있습니다.
class XMLExportVisitor implements Visitor is
    method visitDot(d: Dot) is
        // 점의 아이디와 중심 좌표를 내보냅니다.

    method visitCircle(c: Circle) is
        // 원의 아이디, 중심 좌표 및 반지름을 내보냅니다.

    method visitRectangle(r: Rectangle) is
        // 사각형의 아이디, 왼쪽 상단 좌표, 너비 및 높이를 내보냅니다.

    method visitCompoundShape(cs: CompoundShape) is
        // 모양의 아이디와 그 자식들의 아이디 리스트를 내보냅니다.


// 클라이언트 코드는 요소의 구상 클래스들을 파악하지 않고도 모든 요소 집합 위에서
// 비지터의 작업들을 실행할 수 있습니다. `accept` 작업은 비지터 객체의 적절한
// 작업으로 호출을 전달합니다.
class Application is
    field allShapes: array of Shapes

    method export() is
        exportVisitor = new XMLExportVisitor()

        foreach (shape in allShapes) do
            shape.accept(exportVisitor)
```

# 참고

|내용|URL|
|:---|:---|
|비지터 패턴|https://ko.wikipedia.org/wiki/%EB%B9%84%EC%A7%80%ED%84%B0_%ED%8C%A8%ED%84%B4|
|비지터 패턴|https://refactoring.guru/ko/design-patterns/visitor|