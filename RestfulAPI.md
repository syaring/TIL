# RESTful API

- **[REST](https://en.wikipedia.org/wiki/Representational_state_transfer)**  
  REpresentational State Transfer  
  HTTP를 기반으로 제약조건과 속성을 정의한 아키텍처 스타일
- **[API](https://en.wikipedia.org/wiki/Application_programming_interface)**  
  Application Program Interface  
  소프트웨어를 만들 때 필요한 서브루틴 정의, 프로토콜, 도구 등의 집합  

- REST의 6가지 원칙(제약조건)  

  1. Client-Server (의존성 낮아짐)
  2. Stateless (구현이 단순해짐)
  3. Cacheable (자원 절약 효과)
  4. Layered System (유연성 높아짐)
  5. Uniform Interface (확장성 높아짐)
  6. Code on demand


- REST + API = RESTful API



- REST의 구성  = 자원 + 행위 + 표현  
    GET     /vanillacoding/members/syaring  
    <행위>                          <자원>



- [URI 규칙](https://restfulapi.net/resource-naming/)  

  - 자원의 표현  

    1. 명사 사용

    2. 계층관계 표시 (/)

    3. 가독성 : - (o), _ (x)

    4. 확장자 표시 안함

    5. 소문자로 통일 (혼란 방지)

       /vanillacoding/**showmembers**/3thmember/syaring/profile.**jpg**/  (x)  
       /vanillacoding/members/3/syaring/profile  (o)  
       *참고) Accept header ->  image/jpg*  
       /vanillacoding/members-3th/syaring  (o)  
       /vanillacoding/members**_3TH**/syaring  (x)  
       

  - 자원의 관계 표현  
    /vanillacoding/members/**{meberid}**/profile  

  - 행위  
    HTTP Method로 표현 (GET / POST / PUT / DELETE)
    
  - 표현  
    Document : 문서, 객체  
    Collection : Document의 집합  
    => 모두 resource로, URI 에 표현

- HTTP 응답 상태 코드  
  ![스크린샷 2018-06-25 오후 4.43.57](/Users/syaring_mac/git/TIL/스크린샷 2018-06-25 오후 4.43.57.png)세분화하는 것을 권장

- RESTful API 의 장/단점  

  - 표준이 없음 (유효성 부재 가능)
  - 1 : N (서버 : 클라이언트) 구조에서 효율성이 높음
  - API 메시지만으로도 API를 이해할 수 있음 (Self-descriptiveness)
  - 확장성, 범용성, 독립성 등..



- References  
  [restfulAPI.net](https://restfulapi.net/)  
  [RESTful API 제대로 알고 사용하기](http://meetup.toast.com/posts/92)  
  [Restful API 란 무엇인가](http://joonyon.tistory.com/13)  
  