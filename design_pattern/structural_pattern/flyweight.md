
# 플라이웨이트(Flyweight)

-  각 개체에 모든 데이터를 유지하지 않고 공통된 데이터 공유하여 사용할 수 있게 하는 패턴
> 대표적으로 폰트 같이 변하지 않지만 많이 쓰고, 용량이 큰 객체를 플라이웨이트 패턴을 통해 관리하면 좋다

# 특징
- 컨텍스트의 내재된 상태(intrinsic state)와 외적인 상태(extrinsic state)로 구분하여 내재된 상태에 해당하는 객체를 관리한다
- 컨텍스트 : 내재된 상태와 외적인 상태를 가지고 있는 객체
- 내재된 상태 : 객체별로 다르지 않은 것(ex : 폰트)(불변성)
- 외적인 상태 : 객체별로 다른 것(ex: 폰트로 렌더링할 텍스트)(가변성)
> 싱글톤 패턴과의 가장 큰 차이점은 불변성 여부이다

# 장점
- 한번 생성된 공유 객체를 활용함으로 객체 생성하는 비용을 줄일 수 있다
- 메모리를 절약 할 수 있다

# 단점
- 플라이웨이트 메서드를 호출할 때마다 CPU에 의존하는 계산이 크다면 메모리를 절약하는 대신 퍼포먼스를 희생한 것일 수 있다
- 코드가 복잡해진다

# 예제
나무를 화면에 렌더링할 때 모든 텍스처를 단독으로 사용한다면 수십개는 몰라도 수백 수천개가 되면 메모리가 남아나지 않을 것이다
하지만 플라이웨이트 패턴을 통해 공통된 텍스처를 공유한다면 수만개 이상의 나무도 렌더링 가능하다
```java
// 플라이웨이트 클래스는 트리의 상태 일부를 포함합니다. 이러한 필드는 각 특정
// 트리에 대해 고유한 값들을 저장합니다. 예를 들어 여기에서는 트리 좌표들을 찾을
// 수 없을 것입니다. 그러나 많은 트리들이 공유하는 질감들과 색상들은 찾을 수 있을
// 것입니다. 이 데이터는 일반적으로 크기 때문에 각 트리 개체에 보관하면 많은
// 메모리를 낭비하게 됩니다. 대신 질감, 색상 및 기타 반복되는 데이터를 많은 개별
// 트리 객체들이 참조할 수 있는 별도의 객체로 추출할 수 있습니다.
class TreeType is
    field name
    field color
    field texture
    constructor TreeType(name, color, texture) { ... }
    method draw(canvas, x, y) is
        // 1. 주어진 유형, 색상 및 질감의 비트맵을 만드세요.
        // 2. 비트맵을 캔버스의 X 및 Y 좌표에 그리세요.

// 플라이웨이트 팩토리는 기존 플라이웨이트를 재사용할지 아니면 새로운 객체를
// 생성할지를 결정합니다.
class TreeFactory is
    static field treeTypes: collection of tree types
    static method getTreeType(name, color, texture) is
        type = treeTypes.find(name, color, texture)
        if (type == null)
            type = new TreeType(name, color, texture)
            treeTypes.add(type)
        return type

// 콘텍스트 객체는 트리 상태의 공유된 부분을 포함합니다. 이러한 부분들은 두 개의
// 정수로 된 좌표와 하나의 참조 필드만 참조하여 크기가 작기 때문에 하나의 앱이
// 이런 부분을 수십억 개씩 만들 수 있습니다.
class Tree is
    field x,y
    field type: TreeType
    constructor Tree(x, y, type) { ... }
    method draw(canvas) is
        type.draw(canvas, this.x, this.y)

// Tree 및 Forest 클래스들은 플라이웨이트의 클라이언트들이며 Tree 클래스를 더
// 이상 개발할 계획이 없으면 이 둘을 병합할 수 있습니다.
class Forest is
    field trees: collection of Trees

    method plantTree(x, y, name, color, texture) is
        type = TreeFactory.getTreeType(name, color, texture)
        tree = new Tree(x, y, type)
        trees.add(tree)

    method draw(canvas) is
        foreach (tree in trees) do
            tree.draw(canvas)
```

# 참고

|내용|URL|
|:---|:---|
|플라이웨이트 패턴|https://ko.wikipedia.org/wiki/%ED%94%8C%EB%9D%BC%EC%9D%B4%EC%9B%A8%EC%9D%B4%ED%8A%B8_%ED%8C%A8%ED%84%B4|
|플라이웨이트 패턴|https://refactoring.guru/ko/design-patterns/flyweight|