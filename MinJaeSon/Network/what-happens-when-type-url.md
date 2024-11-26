# 주소창에 url을 입력하면 일어나는 일

브라우저의 주소창에 google.com을 입력한다고 가정해보자. 구글 페이지가 우리 눈에 보이기까지 어떤 일이 발생할까? 그 흐름에 대해 한 번 알아보자.

<br/>

## 1. 도메인 이름으로 DNS 서버에서 IP 주소를 조회한다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnkK0b%2FbtsBiVXiSgI%2FJ5XmWQBkqKipLKe97VZueK%2Fimg.png' alt='url' width='400px'/>

여기서 도메인은 `www.google.com`이 된다. 모든 웹 사이트는 고유한 주소를 가지는데, 실제로는 `142.250.196.110`와 같이 숫자로 이루어져 있고, 이를 **IP 주소**라고 한다. IP 주소를 사람이 이해하고 기억하기 쉽도록 이름을 부여한 것을 **도메인(Domain)**이라고 한다.

따라서 우리는 url의 도메인명을 통해 요청 받은 사이트를 호스팅하는 실제 주소(=IP 주소)를 알아내야 하고, 여기에 관여하는 것이 바로 **DNS(Domain Name Server)**이다. DNS 서버는 웹 사이트의 IP 주소와 도메인 주소를 연결해주는 시스템으로, 도메인을 IP 주소로 변환해주는 역할을 한다.

DNS 데이터는 **브라우저 사이의 여러 계층과 위치에 캐시**되는데, DNS 조회를 하려면 다음 네 곳에서 캐시된 기록을 확인해야 한다.

- **브라우저 캐시 확인**
- **OS 캐시 확인**
- **라우터 캐시 확인**
- **ISP 캐시 확인**

<br/>

## 2. 만약 요청한 주소가 캐시에 없다면 ISP의 DNS 서버에 DNS 쿼리를 보낸다.

**DNS 쿼리**는 원하는 IP 주소를 찾을 때까지 인터넷에서 여러 DNS 서버를 검색하기 위해 사용된다. 즉, 재귀적으로 검색이 이루어지게 된다. -> **재귀적 질의**

![dns architecture](https://velog.velcdn.com/images/forest_xox/post/74033bb6-7f91-4a8c-84dc-f5436236f4cc/image.png)

<br/>

## 3. 브라우저가 서버와 TCP 연결을 한다.

브라우저가 IP 주소를 받았다면 그 주소와 일치하는 서버와 연결하여 정보를 전송해야 한다.
브라우저는 **인터넷 프로토콜(IP, Internet Protocol)**을 사용하여 이러한 연결을 구축하는데, 일반적으로 HTTP(HyperText Transfer Protocol) 요청에서는 **TCP(Transmission Control Protocol)**라는 **전송 제어 프로토콜**을 사용한다.

<br/>

> 💡 IP는 **주소**로서 데이터를 목적지까지 보내는 **길 안내 역할**을 하고, TCP는 **운송 서비스**로서 데이터가 손실 없이 잘 도착하도록 **배송하는 역할**을 한다고 이해하면 쉽다.

<br/>

### 3-way handshake

클라이언트와 서버 간에 데이터 패킷을 전송하려면 TCP 연결을 해야 하는데, 이 연결은 TCP/IP 3-way handshake라는 과정을 통해 이루어진다. 클라이언트와 서버가 `SYN(synchronize: 연결 요청)` 및 `ACK(acknowledgement: 승인)` 메시지를 교환하여 연결을 설정하는 3단계 프로세스이다.

1. 클라이언트는 인터넷을 통해 서버에 **SYN 패킷**을 보내 새 연결이 가능한지 여부를 묻는다.
2. 서버에 새 연결을 수락할 수 있는 열린 포트가 있는 경우, **SYN/ACK 패킷**을 사용하여 SYN 패킷의 ACK로 응답한다.
3. 클라이언트는 서버로부터 SYN/ACK 패킷을 수신하고, **ACK 패킷**을 전송하여 승인한다.

<br/>

## 4. 브라우저가 서버에 HTTP 요청을 보낸다.

데이터 송수신을 위한 TCP 연결이 설정되었으니, 브라우저는 웹 서버에 `www.google.com`의 페이지를 GET으로 요청한다.

이 요청에는 다음과 같은 부가적인 정보들도 담긴다.

![request_1](/MinJaeSon/Images/request_1.png)
![request_2](/MinJaeSon/Images/request_2.png)

<br/>

## 5. 서버가 요청을 처리 후 응답을 보낸다.

서버에는 웹 서버(ex. Apache, IIS 등)가 포함되어 있는데, 이는 브라우저로부터 받은 요청을 Request Handler에 전달하여 요청을 읽고 응답을 생성한다. (필요 시에는 서버의 정보를 업데이트 한다.) 그런 다음 JSON, XML, HTML 등의 형식으로 response를 클라이언트에 보낸다.

response에는 요청한 웹 페이지와 함께 상태 코드(Status Code), 페이지 캐싱 방법(Cache-Control), 설정할 쿠키 등 부가적인 정보가 담겨있다.

![general](/MinJaeSon/Images/general.png)
![response](/MinJaeSon/Images/response.png)

Status Code는 100번대부터 500번대까지가 될 수 있는데, 200번대는 요청이 성공했음을 의미하여 이 경우는 서버로부터 응답을 성공적으로 받은 경우를 가리킨다.

<br/>

## 6. 브라우저는 받은 응답을 파싱하여 화면에 렌더링한다.

브라우저는 응답으로 받은 HTML을 단계에 걸쳐 화면에 표시한다.

이 렌더링 과정에 대해서는 [브라우저의 렌더링 원리](https://github.com/IT-Cotato/10th-Frontend-Interview-Study/blob/develop/MinJaeSon/Browser/browser-rendering.md)를 참고하면 더 자세히 알아볼 수 있다.

![render](/MinJaeSon/Images/render.png)