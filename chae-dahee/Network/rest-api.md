# [네트워크] REST, REST API, RESTful 특징

## REST 정의

- **RE**presentational**S**tate**T**ransfer
- 월드 와이드 웹 과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 형식
  - 웹 기존 기술, HTTP 프로토콜을 그대로 활용할 수 있어 웹의 장점을 최대활용할 수 있다.
- 클라이언트와 서버 사이의 통신 방식 중 하나이다.
- 자원을 정의하고, 자원에 대한 상태(주소를 지정)를 통신 네트워크 아키텍처의 모음

#### **즉, 자원(Resource)의 표현(Representation)에 의한 상태 전달**

<br/>

자원의 표현? 해당 소프트웨어가 관리하는 모든 것(자원)을 표현하기 위한 이름(표현)
(ex. DB 의 학생정보가 자원 -> `students` 표현)

상태전달? 데이터가 요청되는 시점에서 자원의 상태(정보)를 전달한다.

> HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하도록 설계된 아키텍쳐

\* CRUD Operation : Create 생성 `POST`, Read 조회 `GET`, Update 수정 `PUT`, Delete 삭제 `DELETE`, HEAD header정보조회 `HEAD`

### 왜 REST 가 필요한가?

- 애플리케이션 분리 및 통합
- 다양한 클라이언트의 등장
- 다양한 브라우저와 안드로이폰, 아이폰과 같은 모바일 디바이스에서도 통신을 할 수 있어야 한다
- 타 플랫폼에 대한 지원을 위해 서비스 자원에 대한 아키텍처를 세우고 이용하는 방법 > REST

### REST 구성요소

자원 Resource : URI

- 모든 자원은 고유한 ID 가 존재하고, 이 자원은 서버에 저장된다.
- 자원을 구별하는 ID 는 HTTP URI 이다. (ex. `/student/:std\_id` )
- Client는 URI를 이용해서 자원을 지정하고, 자원의 상태(정보)에 대한 조작을 서버에 요청한다.

행위 Verb : HTTP Method

- HTTP 프로토콜의 메소드 method 사용
- `POST`, `GET`, `DELETE`, `PUT`

표현 Representation of Resource

- Client가 자원의 상태에 대해 조작을 요청하면 서버는 이에 해당하는 응답(Representation)을 전송한다.
- 응답은 자원의 상태(정보)를 전달한다
- **`JSON`, `XML`**, `TEXT`, `RSS` 등으로 표현될 수 있다.

### REST 특징

**서버-클라이언트 구조** (Server-Client)

- 자원을 요청하는 쪽이 Client, 자원이 있는 쪽이 Server가 된다.
  - Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
  - REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
- 아키텍처를 단순화시키고 작은 단위로 분리함으로써
  - Client-Server가 독립적으로 개선될 수 있도록 해준다.
  - 서로 간 의존성이 줄어든다.

**무상태** (Stateless)

- HTTP 프로토콜과 동일하게 무상태성을 갖는다.
- 요청에서 Client의 context를 Server에 저장하지 않는다.
  - 세션과 쿠키와 같은 정보를 신경쓰지 않아 구현이 단순해진다.
- Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
  - API 서버는 Client의 요청만을 단순 처리
  - 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
  - Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.

**캐시 처리 가능** (Cacheable)

- 웹 표준 HTTP 프로토콜에서 사용하는 기존의 인프라 활용
- 캐싱 기능 : Last-Modified 태그 E-Tag를 이용해 구현 -> 대량의 요청에서 사용
- 빠른 응답시간
- 전체 응답시간, 성능, 서버의 자원 이용률 향상

**계층화** (Layered System)

- Client는 REST API Server만 호출한다.
- REST Server는 다중 계층으로 구성될 수 있다.
  - API Server 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가해 구조상 유연성
  - 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상
- PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.

**Code-On-Demand(optional)**

- Server로부터 스크립트를 받아서 Client에서 실행한다.
  - Java applet이나 JavaScript를 제공해 Server가 Client가 실행시킬 수 있는 로직을 전송
- 반드시 충족할 필요는 없다.

**인터페이스 일관성** (Uniform Interface)

- URI로 지정한 Resource에 대한 조작은 통일되고 한정적인 인터페이스로 수행한다.

**중요 원칙!!**

- URI는 정보의 자원을 표현해야 한다.
- 자원의 대한 행위는 HTTP method로 표현한다.

<br/>

### REST 장단점

장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능하다.
- API가 의도하는 메시지를 쉽고 명확하게 파악할 수 있다.
- Server와 Client의 역할을 명확하게 분리한다.

단점

- 표준이 존재하지 않는다.
- 사용할 수 있는 method가 제한적이다.
- 구형 브라우저가 아직 지원하지 못하는 부분이 있다.

## REST API

API : Application Programming Interface 서로 정보를 교환 가능하도록

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FonvFl%2FbtsLAypEpGv%2FMrOcycL1IpwUKre9MK26Jk%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

<br/>

REST의 특징을 기반으로 서비스 API를 구현하는 것

최근 OpenAPI나 마이크로 서비스를 제공하는 기업 대부분은 REST API를 제공한다.

**각 요청이 어떤 정보나 동작을 위한 것인지를 그 요청의 모습(URI) 자체로 추론 가능하다.**

- 사내 시스템 - REST 기반으로 시스템을 분산하여 유지보수, 운용에 편리
- 델파이 클라이언트, 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.

장점

- 확장성, 재사용성
- 유연성
- 독립성

### 설계 원칙

- URI는 명사를 사용한다. (동사 X)
- URI는 소문자로만 구성한다. (대문자 X)
  - 하이픈 `-` 사용한다. (언더바 `_` X)
- CRUD 기능을 나타내는 것은 URI 에 사용하지 않는다.
- `/`로 계층 관계를 표현한다.
- URI 마지막 문자로 `/`를 포함하지 않는다.
- HTTP 응답 상태 코드를 사용한다.
  - 도큐먼트(객체 인스턴스와 유사) 이름 : 단수 명사
  - 컬렉션(서버 디렉터리) 이름 : 복수 명사
  - 스토어(클라이언트 리소스) 이름 : 복수명사
  - `Get /members/1`
- 파일확장자는 URI에 포함하지 않는다.
  - Accept header 을 사용한다.

## RESTful API

REST의 설계 규칙을 잘 지켜서 설계된 API를 RESTful한 API라고 한다.

#### 즉, REST 원리를 따르는 시스템, 서비스를 RESTful 하다고 한다.

<br/>

#### 그렇다면 RESTful 하지 못한 것?

- 경로에 resoure, id 이외의 정보가 들어가는 경우 /member/updateName
- CRUD 기능을 모두 POST 로 처리하는 API
- **SOAP API** - Simple Object Access Protocol : 그 자체로 프로토콜이며, 보안이나 메시지 전송 등에 더 많은 표준들이 정해져있기 때문에 조금 더 복잡하다.

#### 목적

일관적인 컨벤션을 통해 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것

요청을 보내는 주소만으로도 어떤 것을 요청하는 지 파악할 수 있다.

성능이 중요할 경우 필수는 아니다.

---

\# 참고

[https://aws.amazon.com/ko/what-is/restful-api/](https://aws.amazon.com/ko/what-is/restful-api/)

[https://velog.io/@wngkdroqkf441/Frontend-%EA%B8%B0%EC%88%A0-%EB%A9%B4%EC%A0%91-%EB%8C%80%EB%B9%84-RestAPI](https://velog.io/@wngkdroqkf441/Frontend-%EA%B8%B0%EC%88%A0-%EB%A9%B4%EC%A0%91-%EB%8C%80%EB%B9%84-RestAPI)

[https://hahahoho5915.tistory.com/54](https://hahahoho5915.tistory.com/54)
