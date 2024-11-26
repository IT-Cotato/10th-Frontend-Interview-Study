<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcYPn0p%2FbtsKX8ra59X%2F9kpoQ0igvpFEtMhZDZlDQk%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

## 1\. 사용자가 웹 브라우저 주소창에 `www.google.com`을 입력한다.

입력어가 URL 이라면 해당 주소의 페이지를 요청하러 갈 것이고

만약 URL 이 아닌 키워드를 입력했다면 각 브라우저(크롬, 익스플로어, 파이어폭스 등)의 기본 검색엔진을 통해 해당 키워드를 검색 할 것이다.

이때 프로토콜 부분을 생략하고 도메인부분을 입력하게 되면 기본적으로 http로 요청하고, 최신 브라우저는 https 를 기본으로 사용하기도 한다.

## 2\. DNS 서버에 DNS 쿼리를 보낸다.

웹 브라우저는 입력된 URL 을 해석하여 도메인 이름을 IP 주소로 변환하기 위해 요청을 보낸다.

캐싱된 DNS 기록을 통해 도메인 이름과 대응하는 IP 주소를 확인한다.

DNS Domain Name System : 도메인 네임 목록과 도메인 네임과 연결된 IP 주소 등의 리소스를 유지 및 관리한다.

`www.google.com` 도메인 이름을 숫자 IP 주소 `142.250.206.196`

[링크](https://dns.google/)에서 검색하면 확인 가능 - DNS server 는 도메인 이름과 매칭되는 IP 주소를 가지고 있는 데이터 서버이다.

[DNS - MDN Web Docs 용어 사전: 웹 용어 정의 | MDN](https://developer.mozilla.org/ko/docs/Glossary/DNS)

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcvogFF%2FbtsKXAIxpbo%2FjtJej39rwwxZAvZFtJDU50%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

1\. DNS 서버에 도메인 이름을 전달한다. DNS query를 날린다.

2\. 통신사(KT, SKT 등)의 DNS 서버에 도착해서 물어본다.

3\. 만일 통신사의 DNS 서버에 없을 경우, 루트 DNS 서버로 넘어가서 물어본다.

3.1. www.google.**com**  의 .com 을 분류해서, com DNS 서버로 연결한다.

3.2. 최상위 도메인 (TLD) DNS 서버는 (kr, jp) 국가코드 or (.net, .com) 일반 최상위 도메인 코드로 나뉘기 때문에 이 기준으로 구분한다.

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlVILS%2FbtsKWNn7l6Q%2FfWT0l425F55aRbyQjSnCZk%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

4\. 루트 DNS 서버가 연결해준 COM DNS 서버로 와서 도메인 이름을 찾는다.

5\. COM DNS 서버가 도메인 등록 정보를 가지고 있는 권한 있는 DNS 서버로 연결한다.

5.1. 가비아 DNS : 국내 도메인 호스팅을 담당하는 곳. COM DNS 서버에서 가비아 DNS 서버를 연결해줄 것이다.

6\. 가비아 DNS 에 해당 도메인 이름을 가진 IP 주소를 가지고 있다. IP 주소를 반환한다. `142.250.206.196`

## 3\. IP 주소로 웹서버에 TCP 3-way handshake 로 연결을 수립한다.

브라우저는 반환된 IP 주소, 즉 해당 서버에 TCP/IP 프로토콜을 사용하여 연결을 시도한다.

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb00yt9%2FbtsKWIf9eZZ%2FLEiNI9AtfqzMea4Yj0r5Lk%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

<br/>
3-way handshake 를 이용해 연결한다.

3-way [handshake](https://developer.mozilla.org/ko/docs/Glossary/TCP_handshake) : TCP 프로토콜을 사용하여 클라이언트와 서버 간의 연결을 설정하는 과정

1\. SYN (Synchronize): 클라이언트가 서버에 연결 요청을 보낸다. 이때 클라이언트는 SYN 플래그가 설정된 패킷을 전송하고, 이 패킷에는 클라이언트의 초기 시퀀스 번호(ISN)가 포함된다.

2\. SYN-ACK (Synchronize-Acknowledgment):

2.1. 서버는 클라이언트의 SYN 패킷을 수신하고, 연결 요청을 수락한다.

2.2. 서버는 SYN-ACK 플래그가 설정된 패킷을 클라이언트에게 보낸다. 이 패킷에는 서버의 초기 시퀀스 번호(ISN)와 클라이언트의 SYN에 대한 확인 응답(Acknowledgment) 번호가 포함된다. 확인 응답 번호는 클라이언트의 ISN + 1로 설정된다.

3\. ACK (Acknowledgment): 클라이언트는 서버의 SYN-ACK 패킷을 수신하고, 연결이 성공적으로 설정되었음을 확인하는 ACK 패킷을 서버에 보낸다. 이 ACK 패킷에는 서버의 ISN + 1이 포함된다.

## 4\. 클라이언트는 웹서버로 HTTP 요청메시지를 통해 html 문서를 요청한다.

http 요청메시지 형식

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fmfu95%2FbtsKWhQu5xg%2FhcPM3lcCifChX5INWPugKK%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FweLiM%2FbtsKWKdY1Wm%2FpJMCfhFKFfKuSOXIMh6Nik%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

TCP 통신을 통해서 `142.250.206.196`의 구글 서버에 GET 요청을 하게 된다.

### 웹 어플리케이션서버(WAS)와 데이터베이스에서 우선 웹페이지 작업을 처리 후 서버에 전송

웹서버 혼자서 모든 로직을 수행하고 데이터를 관리할 수 있다면 간단하겠지만, 그렇게 될 경우 서버에 과부하가 일어날 수 있다.

그렇기 때문에 서버의 일을 돕는 조력자 역할을 하는 것이 웹 어플리케이션이다

WAS는 사용자의 컴퓨터나 장치에 웹 어플리케이션을 수행해주는 미들웨어를 말한다.

브라우저로부터 요청을 받으면, 웹서버는 페이지의 로직이나 데이터베이스의 연동을 위해서 WAS에게 이들의 처리를 요청한다. 그러면 WAS는 이 요청을 받아 동적인 페이지처리를 담당하고, DB에서 필요한 데이터 정보를 받아서 파일을 생성한다.

## 5\. 웹 서버는 HTTP 응답 메시지를 보낸다.

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUtdfc%2FbtsKW44o1N5%2FkzxF11lXtlBm8clNkCFp10%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

응답 페이지와 함께 HTTP 응답 메시지로 보내준다.

status code로 서버요청에 따른 상태를 보낸다.

[상태코드 - 닷다라다나닷](https://datdaradanadat.tistory.com/220#Response%20Status%20Code%20%EC%9D%91%EB%8B%B5%20%EC%83%81%ED%83%9C%20%EC%BD%94%EB%93%9C-1)

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcsARZX%2FbtsKWhQvv1D%2FdQjWtKxpEpiuZxKltwwUYk%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

## 6\. 웹 브라우저 출력

위 html 데이터는 웹 페이지 데이터로 변환되고, 웹 브라우저에 의해 출력된다.

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd7b8v6%2FbtsKVJteOIz%2FBXB7K8VE8WwmFW7edhaPz1%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

1\. html 골격 렌더링, HTML 태그를 확인하고 이미지, CSS 스타일시트, 자바스크립트 파일 등과 같은 웹 페이지의 추가 요소에 대한 GET 요청을 보낸다.

- 정적 파일(Static File)은 일반적으로 브라우저에서 캐싱되므로 다음에 페이지를 방문할 때 다시 가져올 필요가 없다. 단 브라우저의 캐시 정책에 따라 다를 수 있다.

  1.1 html 파싱해서 DOM 트리를 구축

  1.2. CSS파일 링크를 찾아 CSS파일을 받아오고, CSS 오브젝트 모델을 만든다.

2\. DOM 트리와 CSS 오브젝트 모델을 통해 RenderTree 렌더 트리를 만든다.

3.렌더트리를 이용해서 각각의 노드들의 위치를 지정하는 레이아웃 과정을 거친다.

4\. 화면 paint 그린다.

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FP0OSr%2FbtsKXIT4YUf%2FcM03d628Xgeocxl2DGY54K%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >
