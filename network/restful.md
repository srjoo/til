# REST?
- REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것
> HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
> HTTP **Method(POST, GET, PUT, DELETE, PATCH 등)**를 통해
> 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

# REST의 구성 요소
1. 자원 : URI로 구분
2. 행위 : Http Method로 구분(C:POST, R:GET, U:PUT, D:DELETE)
3. 표현 : HTTP Message Pay Load

# REST의 특징
- 서버-클라이언트 구조
- Stateless(무상태) : URI에 CRUD에 대한 구분이 들어가 있지 않다(어떤 자원인지만 명시됨)
- Cacheable(캐시 처리 가능)
- Layered System(계층화)
- Uniform Interface(인터페이스 일관성)

# REST의 장점
- Http를 사용하므로 호환성이 좋다
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다
- 서버와 클라이언트의 역할을 명확하게 분리

# REST의 단점
- 표준이 존재하지 않는다
- Http Method가 제한적이다

# REST API
- RESPT API란 REST의 원리를 따르는 API
> 기본적인 룰이 있는데
> 1. URI는 동사보다는 명사를, 대문자보다는 소문자 (http://localhost/api/run)
> 2. URI의 마지막에 / 를 쓰지 않는다
> 3. "_"(언더바) 대신 "-"(하이픈)을 사용한다
> 4. 파일의 경우 확장자를 쓰지 않는다 (http://localhost/api/run/afile)
> 5. 행위를 표현하지 않는다 (http://localhost/api/run/afile/delete)

# RESTful API
- RESTful이란 REST의 원리를 따르는 시스템을 의미합니다
> 하지만 REST를 사용했다 하여 모두가 RESTful은 아니다
> REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful 하다라고 표현한다
> 모든 CRUD 기능을 POST로 처리 하는 API 혹은 URI 규칙을 올바르게 지키지 않은 API는 REST API의 설계 규칙을 올바르게 지키지 못한 시스템은 REST API를 사용하였지만 RESTful 하지 못한 시스템
