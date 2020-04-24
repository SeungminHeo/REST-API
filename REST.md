REST 란?
---
정리하는 내용은 Naver Deview 2017, "그런 REST API로 괜찮은가, 이응준 님" 강연을 참고하였습니다. 

웹과 관련된 역사
---

### WEB
- 인터넷에서 정보를 공유하는 
     - 표현방식 : HTML
     - 식별자 : URI
     - 전송방법 : HTTP
     
- Roy T. Fielding
    - 웹에 문제(호환성 등)를 일으키지 않고 HTTP 기능을 향상시킬 수 있을까?
        - HTTP Object Model란 이름의 처음 등장
        - REST의 등장

- REST by Roy T. Fielding, Microsoft Research 
    - [Representational State Transfer : An Architectural Style for Distributed Hypermedia Interaction(1998)](https://roy.gbiv.com/talks/webarch_9805/index.htm)
    - [Architectural Styles and the Design of Network-based Software Architectures(2000)](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
    
### API
- XML-RPC(1998) by Microsoft -> SOAP

- SOAP vs REST
    - SOAP 
        - 복잡, 규칙이 많음, 어렵다
    - REST
        - 단순, 규칙이 적음, 쉽다?
        - [Microsoft REST API Guidelines(2016)](https://github.com/Microsoft/api-guidelines)
            - ~~이건 REST API 가 아니다 by Roy T Fielding~~


REST
---
REST: 아키텍쳐 스타일
- REST : 분산 하이퍼미디어 시스템(ex. 웹) 을 위한 아키텍쳐 스타일
    - 아키텍쳐 스타일 : 제약조건의 집합
    
- [REST를 구성하는 스타일](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) : REST는 하이브리드 아키텍쳐 스타일 = 그 자체로서도 스타일이면서 스타일들의 집합임.
    - client-server
    - stateless
    - cache
    - _uniform interface_ : 제일 어려운 제약조건
    - layered system
    - code-on-demand(optional) : javascript
    
- uniform interface
    - identification of resources
    - manipulation of resources through representations
    - *self-descriptive messages* : 메시지로 모든 것을 설명해야한다. 
        - 목적지(host), return 메시지가 어떤 문법, 의미, 역할로 작성되어있는지 알 수 있어야 함.
    - _hypermedia as the engine of application status(HATEOAS)_ : 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다.
        - html a tag 의 경우 잘 지켜줌. 
        - json의 경우 link 헤더 사용
    - 왜 uniform interface가 필요한가? A. 독립적 진화
        - 서버와 클라이언트가 독립적으로 진화 가능하다.
            - 서버의 기능 변화가 클라이언트에 영향을 미치지 않는다.
        - HTTP에서 문제를 일으키 않도록 해야한다는 초기 목적에 부합함.
        - 웹페이지, 웹브라우저, HTML, HTTP 의 변화가 각각 독립적이기 때문에 하나의 변화가 다른 변화를 야기하지 않음.
            - ~~하지만 호환성이 영원히 보장되지는 못함~~
        - 이 모든 것은 노력에 의한 결과물. Ex. 상호운용성에 대한 집착
            - referer과 같은 오타, charset과 잘못된 이름 등을 수정하지 않음

- 결론적으로, 웹의 독립적 진화에 REST가 기여하게 됨
    - HTTP에 대한 지속적인 영향
    - Host 헤더의 추가
    - URI 길이 제한 방법 명시: 414 에러 등
    - URI 리소스의 정의가 추상적으로 변화함: 식별하고자 하는 무언가
    - HTTP, URI에 영향을 많이 줌
    - 저자 Roy T.Fielding: HTTP, URI 명세의 저자
         

REST API
---
REST API의 경우 REST 아키텍쳐를 따르지 않는 경우가 많음.
- REST API : 상태 전이를 지시하는 하이퍼텍스트가 포함된 uniform interface, self-descriptive한 메시지를 네트워크 기반으로 전달하는 API
- REST는 통제불가능한(?) 시스템을 유지하기 위한 진화에 강조를 두고 있음
    - API를 구성함에 있어 시스템을 통제할 수 있고 진화에 고려가 없다면 굳이 REST일 필요는 없다고 함.
    - ~~실제로 대부분은 REST API가 아니다.~~
- 웹에 비해 API에서 지켜지기 어려운 이유
    - JSON으로 인해 발생

| | Web | HTTP API |
|:---:|:---:|:---:|
|protocol|HTTP|HTTP|
|커뮤니케이션|사람-기계|기계-기계|
|Media Type|HTML|JSON|

- HTML vs. JSON
    - HTML의 경우, Content-Type을 통해 Media-Type을 보고 HTTP 명세에 media-type이 IANA에 등록되어있으므로 찾아서 해석하면 됨.
        - HATEOAS도 a 태그를 통해 정의가 가능함
    - JSON의 경우, 같은 방식으로 Content-Type을 보고 IANA를 통해 파싱이 가능하지만, 내부 정보가 무엇인지 알 수가 없음.
        - HATEOAS의 경우도 링크가 없어서 상태전이가 안됨.
        
    
| | HTML | JSON |
|:---:|:---:|:---:|
|하이퍼링크|가능|정의되어있지 않음|
|self-descriptive|가능(by 명세)|불완전하게 정의(문법은 있지만 값에 대한 정의는 없음)|

- *Self-Descriptive* : 확장 가능한 커뮤니케이션
    - 서버나 클라이언트가 변해도 언제나 Self-Descriptive 하다는 점 때문에 언제나 해석이 가능하다.
- HATEOAS : 애플리케이션 상태 전이의 latebinding
    - 전이하는 목적지는 미리 결정되는 것이 아니라, 해당 하이퍼링크로 전이 된 후에 확인하고 결정된다.
    
#### Self-Descriptive한 REST API?
- Media Type 세부적으로 정의하여 세부 내용에 대한 정의까지 IANA에 등록한다. (?????? 너무 번거롭다.)
- Profile을 틍록한다. Documentation을 통해 해석하는 방법을 전달한다.
    - 단점 : 클라이언트가 link 헤더, profile relation을 이해해야함. Contents negotiation이 안됨.

#### HATEOAS ?
- 데이터에 직접 적시하여 하이퍼링크를 표현한다.
    - 링크 표현 방식에 대한 자체적인 정의가 필요함.
- JSON으로 하이퍼링크를 표현하는 명세를 활용한다(JSON API, HAL 등)
- 데이터와 헤더를 모두 활용해야한다.





    
 
        
            
    