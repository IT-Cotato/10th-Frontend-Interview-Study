### REST API

REST 원칙을 적용하여 서비스 API를 설계한 것

### REST(REpresentational State Transfer)

- 전반적인 웹 어플리케이션에서 상호작용하는데 사용되는 웹 아키텍쳐 모델
- 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것
- HTTP URI를 통해 자원을 명시하고 HTTP 메서드(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD를 적용하는 것
- HTTP 메서드를 통해 이를 처리한다.

### 구성요소

- 자원(Resource) : HTTP URI
- 자원에 대한 행위(Verb) : HTTP Method
- 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

### REST의 특징

- Server-Client(서버-클라이언트 구조)

  REST 서버: API제공 / 클라이언트: 사용자 인증이나 컨텍스트(세션,로그인 정보)를 직접 관리하는 구조로 구분된다.

  → 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어든다.

- Stateless(무상태)

  작업을 위한 상태정보를 따로 저장하고 관리하지 않는다.

  세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API서버는 들어오는 요청만을 단순히 처리한다.

  때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.

- Cacheable(캐시 처리 가능)

  HTTP라는 기존 웹 표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능하다.

  따라서 HTTP가 가진 캐싱 기능이 적용 가능하다.

  HTTP프로토콜 표준에서 사용하는 LAST-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능하.

- Layered System(계층화)

  보안로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수있다.

  PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있다.

- Uniform Interface(인터페이스 일관성)

  URI로 지정한 리소스에 대한조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일

**장점**

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

**단점**

- 표준이 자체가 존재하지 않아 정의가 필요하다.
- HTTP Method 형태가 제한적이다.
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
- 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스폴로어)

---

### API(Application Programming Interface)

- 응용프로그램에서 사용할 수 있도록 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스
- 즉, 프로그램끼리 통신할 수 있도록 하는 중재자
- 구글 맵 API, 카카오 비전 API 등 기존에 있는 응용 프로그램을 통해서 데이터를 제공받거나 기능을 사용하고자 할 때 사용하는 인터페이스 및 규격
- API는 프로그래밍 언어, 운영체제 등에서도 사용되는 범용적인 용어이다.
- REST API라는 것은 REST 원칙을 적용하여 서비스 API를 설계한 것을 말하며 대부분의 서비스가 REST API를 제공한다.

### URI(Uniform Resource Identifier)

- 리소스(전화,지도,이미지,텍스트)에 접근할 수 있는 유일한(Uniform) 식별자(Identifier)
- URI를 수신하는 기기는 해당 URI에 맞게 데이터를 반환한다.

### CRUD

- 대부분의 sw가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말
- 사용자 인터페이스가 갖추어야 할 기능(정보의 참조/검색/갱신)을 가리키는 용어로서도 사용된다.

---

**참조**

**[https://velog.io/@yhlee9753/REST-API-%EC%99%80-HTTP-Stateless-%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0](https://velog.io/@yhlee9753/REST-API-%EC%99%80-HTTP-Stateless-%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)**

[https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/rest-api.md](https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/rest-api.md)

[https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)
