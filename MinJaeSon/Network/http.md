# HTTP란?

**Hyper Text Transfer Protocol**의 약자로, HTML과 같은 하이퍼미디어 문서를 전송하기 위한 애플리케이션 계층의 프로토콜이다. 쉽게 말해 **서버와 클라이언트 간에 데이터를 주고 받기 위해 사용되는 통신 규약**이라고 이해하면 된다.

<img src='https://velog.velcdn.com/images/xgro/post/a6003bf5-647d-4f87-b363-98d020306f4a/image.png' alt='network layer' width='400px'/>

<br/>

문서뿐 아니라 이미지, 영상, 파일, JSON, XML 등 거의 모든 형태의 데이터를 전송할 수 있어 대부분 HTTP 프로토콜을 사용하여 통신을 한다.

<img src='https://blog.kakaocdn.net/dn/dxHA8s/btrdespYYeJ/XKXkbnx1eofhdMd8mHQgyk/img.png' alt='http communication' width='400px'>

**HTTP 통신**은 클라이언트(Frontend)와 서버(Backend)로 단이 나뉘어 **클라이언트가 요청(request)을 하면 서버가 이에 대한 응답(response)을 보내주는 방식**으로 동작한다.

<br/>

> 💡 **URL에서의 http**
>
> 예를 들어 http://www.cotato.kr 라는 인터넷 주소가 있다면, www.cotato.kr 이라는 주소가 가진 데이터들을 HTTP 통신 규약대로 처리하라는 것을 의미한다.

<br/><br/>

## HTTP의 역사

#### HTTP 0.9

- GET 메서드만 지원
- http 헤더도 없었음

#### HTTP 1.0

- 메서드, 헤더, 상태코드가 추가됨
- 단기커넥션 : 하나의 connection 당 1 요청, 1 응답만 가능 -> 매번 연결 필요

#### HTTP 1.1

- 현재 가장 많이 사용하며, 대부분의 기능이 이때 추가됨
- 지속적인 커넥션 : 지정한 timeout 동안 연속적인 요청 사이에도 connection을 지속
- Pipelining : 커넥션에 대해 응답까지 기다리지 않고 연속적인 요청을 받아 순차적으로 처리하는 방식 (Head of Line Blocking으로 인해 잘 활용되지 않음)
- Head of Line Blocking : 우선순위가 높은 요청의 시간이 오래 걸리면 다른 요청에 대한 지연이 길어지는 문제 발생

#### HTTP 2.0

- 2.0부터는 성능 개선에 초점이 맞춰짐
- Multiplexing : 하나의 TCP 연결에서 여러 요청과 응답을 동시에 처리할 수 있게 되어 Head of Line Blocking 문제를 보완
- HPack : 헤더 압축 및 중복값 개선

#### HTTP 3.0

- TCP 대신 UDP 기반의 QUIC 프로토콜을 사용함 -> TCP의 지연시간(RTT) 문제를 해결

<br/><br/>

## HTTP의 특징

### 무상태성(stateless)

서버가 클라이언트의 상태를 유지하지 않음을 의미한다. <br/>
따라서 필요한 상태에 대한 정보는 클라이언트가 갖게 하여, 클라이언트의 요청에 대해 어느 서버가 응답을 해도 상관이 없어진다.

-> 장점 : 서버의 Scale Out에 유리하다 <br/>
-> 단점 : stateful보다 데이터를 더 많이 사용한다

<br/>

### 비연결성(connectionless)

서버와 클라이언트 간 연결을 지속하지 않음을 의미한다. <br/>
클라이언트가 서버에 요청을 하고 응답을 받으면 즉시 TCP/IP 연결을 끊는다.

-> 장점 : 서버의 자원을 효율적으로 관리하고, 수많은 요청들에 대응할 수 있다 <br/>
-> 단점 : 매번 연결을 위한 TCP handshake를 맺어야 한다

<br/>

> 💡 비연결성과 HTTP 지속 연결
>
> 초기에는 이 비연결성으로 인해 모든 자원에 대해 TCP Handshake가 발생하여 요청 시간이 길어지는 문제점이 있었는데, 필요한 자원을 모두 다운받을 때까지 연결을 유지하도록 하여(Persistent Connection) 한계점을 극복하였다.

<br/><br/>

## HTTP 상태 코드

HTTP 상태 코드는 클라이언트가 보낸 요청에 대한 처리 상태를 응답에서 알려주는 코드이다.

<img src='https://velog.velcdn.com/images%2Fej_shin%2Fpost%2Fb73b1fa9-6654-4021-bb06-0cbec2577896%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-02%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.52.31.png' alt='http status code' width='500px'/>

<br/><br/>

## HTTP 메시지

HTTP Request Message는 클라이언트가 서버에 요청하는 메시지이고, HTTP Response Message는 서버가 클라이언트에 응답하는 메시지이다.

개발자 도구의 네트워크 탭을 통해 확인해 볼 수 있는데, 여기에 담겨 있는 정보가 무엇인지 알면 서버와 데이터를 송수신 할 때 많은 도움을 얻을 수 있다.

<img src='https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd358411c-d9f8-4c8b-b0dc-3465ca72d49c%2FScreen_Shot_2023-06-26_at_3.16.56_PM.png&blockId=6435051b-a1c0-49b1-badb-ada2a7962c31' alt='http message structure' width='360px'/>

기본적으로 위와 같이 순차적으로 구성되어 있다.

참고로 공백 라인은 header와 body를 구분하기 위함으로, body가 비어있다고 해도 반드시 넣어주어야 한다.

<br/>

### HTTP 요청 메시지

- Start Line

  Method : HTTP 메서드 <br/>
  URL : 요청 대상의 경로 <br/>
  Version : 사용된 http 버전

- Header

  Headers : HTTP 전송에 필요한 모든 부가 정보 (ex. 메세지 크기, 압축 여부, 인증, 브라우저 정보, 서버 정보, 캐시 등)

- Empty Line

- Message Body

  Message Body : 실제 전송할 데이터 (ex. HTML 문서, 이미지, 영상, JSON 등)

<br/>

### HTTP 응답메시지

- Start Line

  Version : 사용된 http 버전 <br/>
  Status Code : 요청에 대한 상태 코드 <br/>
  Status Message : 상태에 대해 설명하는 메시지

- Header

  Headers : HTTP 전송에 필요한 모든 부가 정보 (ex. 메세지 크기, 압축 여부, 인증, 브라우저 정보, 서버 정보, 캐시 등)

- Empty Line

- Message Body

  Message Body : 전송 받은 데이터

<br/><br/>

## HTTP Method

주요 메서드 5가지

**`GET`** : 리소스 조회

**`POST`** : 요청 데이터 처리, 주로 데이터 등록에 사용

**`PUT`** : 리소스를 대체, 해당 리소스가 없을 시 생성

**`PATCH`** : 리소스를 일부만 변경

**`DELETE`** : 리소스 삭제

<br/>

> ✨ **PUT과 PATCH의 차이점**
>
> PUT은 리소스 전체 업데이트, PATCH는 부분 업데이트라는 점에서 차이가 있다.

<br/>

> ✨ **전송 방식에 있어 GET과 POST의 차이점**
>
> GET은 URL에 쿼리 스트링으로 데이터를 포함하여 전송한다.
>
> > ex. `www.example.com/resources?name1=value1&name2=value2`
> >
> > - 내용이 노출되기 때문에 민감한 정보는 담지 않는 것이 좋다.
> > - 요청에 길이 제한이 있다.
> > - HTTP 패킷 헤더에 데이터가 담긴다.
> > - 요청이 브라우저의 히스토리에 남는다.
> > - 불필요한 요청을 제한하기 위해 캐싱이 될 수 있다. (js, 이미지 같은 정적 콘텐츠의 경우 데이터의 크기가 크고 변경될 확률이 적기 때문)
>
> POST는 HTTP body에 데이터를 담아 전송한다.
>
> > - body에 보낼 데이터의 타입은 Header의 Content-Type에서 설정해주어야 한다.
> > - 개발자도구를 통해 확인이 가능하기 때문에 POST 방식 또한 민감한 정보의 경우 암호화를 반드시 해주어야 한다.
> > - HTTP 패킷 바디에 데이터가 담긴다.
> > - 요청에 길이 제한이 없고, 브라우저 히스토리에 기록이 남지 않는다.
> > - 캐시되지 않는다.

<br/>

### HTTP 메서드의 속성

- **멱등성**

  동일한 요청을 여러 번 보내도 한 번 호출한 것과 결과가 달라지지 않는 속성

- **안정성**

  호출해도 리소스가 변경되지 않는 속성

- **캐시성**

  응답 결과 리소스를 캐싱해서 효율적으로 사용할 수 있게 하는 속성

  <br/>

  <img src='https://velog.velcdn.com/images%2Fyounoah%2Fpost%2F7db50cb5-9213-4949-95d5-c311633e6775%2Fimg1.daumcdn.png' alt='http method property' width='300px'/>

<br/><br/>

## HTTP와 HTTPS

HTTPS는 **모든 데이터를 암호화된 상태로 전송**하여 보안성 면에서 HTTP보다 뛰어나다.

<br/><br/><br/>

참고 : <br/>
[HTTP ㅡ MDN Web Docs](https://developer.mozilla.org/ko/docs/Web/HTTP) <br/>
[POST, PUT, PATCH의 차이점 ㅡ 토스페이먼츠](https://docs.tosspayments.com/blog/rest-api-post-put-patch#patch) <br/>
[HTTP는 무엇일까요? - 기본 핵심 요약 총정리](https://inpa.tistory.com/entry/HTTP-%F0%9F%8C%90-%EB%B0%B1%EC%97%94%EB%93%9C-%EB%A1%9C%EB%93%9C%EB%A7%B5-HTTP%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C%EC%9A%94#http%EC%9D%98_%EC%97%AD%EC%82%AC) <br/>
[HTTP의 멱등성 · 안정성 · 캐시성 💯 완벽 이해하기](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-HTTP%EC%9D%98-%EB%A9%B1%EB%93%B1%EC%84%B1-%C2%B7-%EC%95%88%EC%A0%95%EC%84%B1-%C2%B7-%EC%BA%90%EC%8B%9C%EC%84%B1-%F0%9F%92%AF-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0#http_%EB%A9%94%EC%84%9C%EB%93%9C%EC%9D%98_%EC%86%8D%EC%84%B1) <br/>
[HTTP와 HTTPS의 차이점은 무엇인가요? ㅡ AWS](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/) <br/>
[[web] Get과 Post의 차이를 알아보자](https://velog.io/@soopy368/web-Get%EA%B3%BC-Post%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
