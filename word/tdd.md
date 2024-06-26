# TDD(Test-driven development)
- 테스트가 전체 개발을 주도해 내가는 것
- 테스트 코드 작성을 먼저 한다
> 개발자는 먼저 요구사항을 검증하는 자동화된 테스트 케이스를 작성한다. 그런 후에, 그 테스트 케이스를 통과하기 위한 최소한의 코드를 생성한다. 마지막으로 작성한 코드를 표준에 맞도록 리팩토링한다.

# 기존 패러다임 vs TDD
기존 : 설계 >> 개발 >> 테스트(문제시 설계 단계로)
TDD : 설계 >> 테스트(문제시 설계 단계로) >> 개발

# 장점
- 코드를 작성하기전 설계에 대해 구체적으로 작성이 가능
> 설계를 강제하게 만들며 보다 구체적으로 동작에 대해 서술 함으로써 내가 무엇을 만들어야 하는지 어떤 기능을 만들어야 하는지 명확하게 파악
- 오류에 대해 신속한 파악이 가능하며, 디버깅 시간을 단축
> 결함은 일찍 찾을 수록 고치는 비용이 적게 든다. 테스트 코드를 통해 어떤 부분이 어디까지 통과를 했는지 어느 부분에 문제가 발생했는지 명확하게 알 수 있으므로 신속한 파악 가능
- 문서의 대체 가능

# 단점
- 초기 테스트 코드 작성 시간이 많이 든다
> 익숙해지면 극복가능?
- 100% 안전하지 않다
> 예측 불가능한 상황을 테스트 코드로 만들 수 없다

# 종류
1. A Unit test [단위 테스트]
2. A Widget (or Component) test [컴포넌트 테스트]
3. An Integration test [통합 테스트]

# 단위 테스트
- 단일 함수 또는 클래스를 테스트 하는 것으로 코드의 논리 단위의 정확성을 확인
> 다음은 포함되지 않는다
> [디스크에 쓰고 읽는 것, 화면에 렌더링 되는것, 테스트가 실행되는 프로세스 외부에서 사용자의 입력을 외부에서 받는 것]

# 컴포넌트 테스트
- 하나의 컴포넌트 또는 위젯을 테스트 하는 것으로 UI를 테스트 하는 것

# 통합 테스트
- 완전한 응용 프로그램을 통합적으로 테스트 하는 것

# 각 테스트 별 효율
- 비용이 많이 드는 테스트일 수록 신뢰도가 비례하여 증가한다