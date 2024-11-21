# HTTP Overview

## 웹 브라우저 (클라이언트)와 웹 서버 (서버)

- **HTTP (HyperText Transfer Protocol)**: 브라우저와 서버 간 통신에 사용하는 프로토콜.
- **URL (Uniform Resource Locator)**: 웹에서 특정 리소스를 찾기 위한 주소.
  - 구성: **Host name** (도메인 이름) + **Path name** (파일 경로).

---

# HTTP

## Hypertext Transfer Protocol

- **Client-Server Architecture**를 기반으로 동작:
  - **클라이언트**: HTTP 프로토콜로 서버에 요청을 보내고 응답을 받아 화면에 출력.
  - **서버**: 클라이언트 요청에 대한 응답으로 오브젝트(텍스트, 이미지 등)를 전송.

### HTTP의 주요 특징

- **TCP (Transmission Control Protocol)** 사용:
  - 데이터의 손실 없이 웹 페이지 내용을 전달하기 위해 신뢰성 높은 TCP 기반 전송 사용.
- **Stateless (무상태성)**:
  - 서버는 클라이언트의 이전 요청 상태를 기억하지 않음.
  - 요청 간 독립성을 유지하지만, 필요 시 상태를 기록하기 위해 **쿠키** 등을 사용.

---

# HTTP Connections

## Non-Persistent HTTP (~1.0)

- 하나의 **TCP connection**을 통해 **최대 한 개의 오브젝트**만 전송.
- 전송 완료 후 **connection이 닫힘**.
- 여러 오브젝트를 요청하려면 각 요청마다 새로운 connection이 필요 → **비효율적**.

## Persistent HTTP (1.1~)

- 하나의 **TCP connection**으로 여러 개의 오브젝트 전송 가능.
- 서버가 응답을 보낸 후에도 connection을 유지.
- **Pipe-lining**:
  - 요청에 대한 응답을 기다리지 않고, **한번에 여러 요청을 연속적으로** 보냄.

---

# Response Time

- **RTT (Round-Trip Time)**: 작은 패킷이 클라이언트에서 서버로 갔다가 다시 돌아오는 시간.
  - **TCP connection 생성**: 1 RTT 필요.
  - **HTTP 요청 및 응답 전송**: 1 RTT 추가.
  - **HTTP response 전송 시간**도 포함됨.

### HTTP Response Time 계산

\[
\text{HTTP Response Time} = 2 \times \text{RTT} + \text{Transmission Time}
\]

---

# HTTP Request Message

- **Request**: 클라이언트가 작성하여 서버로 전송.
  - **POST method**: 사용자 input 데이터를 메시지로 전송.
  - **GET method**: 클라이언트가 URL을 통해 서버에 값을 전달.
  - **HEAD method**: 테스트용 요청. 오브젝트 없이 메타데이터만 응답받음.
  - **PUT method**: 서버에 파일 업로드 요청.
- **Response**: 서버가 작성하여 클라이언트로 전송.

---

# HTTP Response Status Codes

- **200 OK**: 요청 성공.
- **301 Moved Permanently**: 요청된 리소스가 영구적으로 이동.
- **400 Bad Request**: 잘못된 요청.
- **404 Not Found**: 요청된 리소스를 찾을 수 없음.
- **505 HTTP Version Not Supported**: HTTP 버전이 지원되지 않음.

---

# Maintaining User/Server State: Cookies

- **Stateless** HTTP는 상태 정보를 저장하지 않으므로, 필요 시 **쿠키**를 사용.
- **Cookie**:
  - 사용자의 컴퓨터(클라이언트)에 저장되는 작은 데이터 조각.
  - 사용자의 상태 정보를 관리하는 데 사용.

### Cookie 동작 과정

1. 클라이언트가 서버로 **Request Message**를 전송.
2. 서버는 고유 ID를 생성해 쿠키에 설정하고 클라이언트로 **Response** 전송.
3. 클라이언트는 이후 요청 시 쿠키 ID를 포함하여 서버로 전송.
4. 서버는 해당 쿠키 정보를 기반으로 상태를 조회하고 **Response**를 생성.

- HTTP는 **쿠키 전달 역할만 수행**.
- **웹 브라우징 추적** 등 다양한 활용 가능.

---

# Web Caches

## Proxy Server

- 클라이언트와 **Origin Server** 사이에 위치.
- 자주 요청되는 데이터를 Proxy에 저장해 **빠른 데이터 제공**.
- 필요 시 Proxy에서 데이터를 가져오므로 Origin Server까지 접근할 필요 없음.

## Cache

- 클라이언트와 서버 역할을 동시에 수행.
- **장점**:
  - Response Time 감소.
  - Origin Server 트래픽 감소.

## Conditional GET

- **Origin Server**의 오브젝트가 변경된 경우 Cache를 업데이트.
- **GET Method**에 조건문 추가:
  - Header에 저장된 **Modified 날짜**와 비교.
  - 변경되지 않았으면 Cache에서 데이터 제공.
  - 변경되었으면 Origin Server에서 최신 데이터 제공 및 Cache 업데이트.

---

# HTTP/2

- **HTTP 1.1**에서 **Multi-Object 처리**를 개선.
- 주요 특징:
  - 빠른 처리 우선 순위 설정.
  - 오브젝트를 **Frame 단위**로 분할.
  - 클라이언트 요청을 **미리 예측**하여 처리.

---

# HTTP/3

- **구글 QUIC** 기반 프로토콜.
- 주요 특징:
  - **TLS** 내장으로 보안 강화.
  - **UDP** 사용으로 속도 개선 및 지연 시간 감소.
