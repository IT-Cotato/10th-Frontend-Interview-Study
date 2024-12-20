# REST와 RESTful 정리

## REST 정의

- REST(Representational State Transfer)는 자원을 정의하고 자원에 대한 작업 방식을 표준화한 아키텍처 스타일임.
- HTTP 프로토콜을 기반으로 설계되며, 자원(Resource), 메서드(Method), 표현(Representation)이라는 세 가지 주요 요소로 구성됨.

---

## REST 원칙

1. **클라이언트-서버 구조**

   - 클라이언트와 서버는 독립적으로 동작해야 함.
   - 클라이언트는 요청, 서버는 응답 처리.

2. **무상태성 (Stateless)**

   - 서버는 클라이언트의 상태를 저장하지 않음.
   - 요청은 독립적이고 필요한 모든 정보를 포함해야 함.

3. **캐시 가능성 (Cacheable)**

   - 응답 데이터는 캐싱 가능해야 하며, HTTP 헤더로 캐싱 정책을 지정해야 함.

4. **계층화 시스템 (Layered System)**

   - 클라이언트와 서버 사이에 여러 계층이 존재 가능.
   - 계층별로 독립적으로 동작하며 보안 및 확장성 제공.

5. **통합된 인터페이스 (Uniform Interface)**

   - 자원은 URI로 명확히 식별되어야 함.
   - 일관된 HTTP 메서드와 표현 형식 사용.

6. **코드 온 디맨드 (Code on Demand, 선택적)**
   - 필요할 경우 서버에서 클라이언트로 코드 전달 가능.
   - 예: JavaScript 코드 전달.

---

## RESTful 정의

- RESTful은 REST 원칙을 준수하며 설계된 API를 의미함.
- 모든 RESTful API가 REST 원칙을 완벽히 따르는 것은 아님.
- 직관적이고 일관성 있는 URI 설계, 적절한 HTTP 메서드 사용, 상태 코드 활용 등이 필요.

---

## RESTful API 설계의 핵심

1. **URI 설계**

   - 자원을 명확히 식별하는 계층적 구조 사용.
   - 예:
     - 모든 사용자 목록: `/users`
     - 특정 사용자: `/users/{id}`
     - 특정 사용자의 게시글: `/users/{id}/posts`

2. **HTTP 메서드**

   - CRUD 작업에 적합한 메서드 활용.
     - GET: 데이터 조회
     - POST: 데이터 생성
     - PUT: 데이터 전체 수정
     - PATCH: 데이터 부분 수정
     - DELETE: 데이터 삭제

3. **표현 형식**

   - JSON, XML 등 다양한 표현 형식 지원.
   - 요청 헤더(`Content-Type`, `Accept`) 기반으로 응답 생성.

4. **상태 코드**

   - HTTP 상태 코드를 적절히 활용.
     - 200 OK: 성공
     - 201 Created: 생성 성공
     - 400 Bad Request: 잘못된 요청
     - 401 Unauthorized: 인증 실패
     - 404 Not Found: 자원 없음
     - 500 Internal Server Error: 서버 오류

---

## REST vs RESTful

- REST: 자원을 정의하고 이를 조작하기 위한 아키텍처 스타일과 제약 조건.
- RESTful: REST 원칙을 준수한 API.
  - 예: `POST /users/delete/123`는 RESTful하지 않음.
  - 올바른 RESTful 방식: `DELETE /users/123`.

---

## RESTful API 설계 예시

1. **자원 URI**

   - `/users`: 사용자 목록
   - `/users/{id}`: 특정 사용자
   - `/users/{id}/posts`: 특정 사용자의 게시글

2. **요청과 응답**
   - 요청:
     ```http
     GET /users/123 HTTP/1.1
     Host: api.example.com
     ```
   - 응답:
     ```json
     {
       "id": 123,
       "name": "John Doe",
       "email": "john.doe@example.com"
     }
     ```

## API

- API(Application Programming Interface)는 애플리케이션 간 상호작용을 가능하게 하는 인터페이스임.
- 클라이언트와 서버 간 데이터를 주고받거나 기능을 사용할 수 있도록 설계됨.
- REST API는 HTTP 프로토콜을 활용해 REST 원칙을 기반으로 설계된 API를 의미함.

---

## CRUD

- CRUD는 Create, Read, Update, Delete의 약자로, 데이터 조작의 기본 작업을 나타냄.
- RESTful API 설계에서 HTTP 메서드와 매핑됨:
  - **Create** → `POST`
  - **Read** → `GET`
  - **Update** → `PUT` (전체 수정), `PATCH` (부분 수정)
  - **Delete** → `DELETE`

### CRUD 예시

- 사용자 정보와 관련된 CRUD 예:
  - **Create**: `POST /users` → 사용자 생성
  - **Read**: `GET /users/{id}` → 특정 사용자 조회
  - **Update**: `PUT /users/{id}` → 특정 사용자 정보 수정
  - **Delete**: `DELETE /users/{id}` → 특정 사용자 삭제

---

## URI, URL, URN 정의 및 차이점

### URI (Uniform Resource Identifier)

- 인터넷에서 자원을 식별하기 위한 문자열.
- **URL**과 **URN**을 포함하는 상위 개념.

### URL (Uniform Resource Locator)

- 자원의 위치를 나타내며, 해당 자원을 접근하기 위한 경로를 포함함.
- **프로토콜(스키마)**, **도메인**(혹은 IP), **경로**로 구성.

### URN (Uniform Resource Name)

- 자원의 고유한 이름을 식별하며, 위치 정보는 포함하지 않음.
- 특정 네임스페이스에서의 고유 식별자.
