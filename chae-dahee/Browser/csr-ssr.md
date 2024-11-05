# 개요

[SSR vs CSR 로 프로젝트 구성하기](https://datdaradanadat.tistory.com/147)

이 기술의 선택과 적용에 고민을 하며 작성한 개발글

이론과 기술면접에 대비해 좀 더 깊게 작성해보려 한다.

SSR, CSR 은 렌더링 하는 방법

SPA, MPA 는 페이지 구성된 어플리케이션 방법

공식은 `CSR - SPA` `SSR - MPA` 로 많이 사용되고, React 의 경우 SPA 를 기본적으로 사용한다고 알고 있다.

그러나 서버측 html 적용 등(node.js...) 추가적인 세팅을 통해 구현하면 SPA - SSR 로 구현할 수 있다.

즉 React 등 JS 기반 라이브러리는 HTML, CSS, JS 파일이 하나씩 나오기 때문에 SPA-CSR 이 되지만,

SSR 로 구현하면 별도 렌더링으로 동작하기 때문에 MPA 가 될 수 있다.

<br/>

# 렌더링

브라우저의 화면에 내용을 표시하는 것.

클라이언트게 접근하면, 서버에 요청을 보낸다.

화면에 그려지는건 HTML 인데,

`SSR` 서버가 렌더링 할 경우, HTML, View 등의 리소스를 해석하고, 렌더링해서 클라이언트에게 반환한다.

`CSR` 클라이언트가 렌더링 할 경우, 서버는 리소스를 제공하고, 클라이언트가 해설하여 렌더링한다.

<br/>

# CSR (Client-side Rendering) - SPA (Single Page Application)

CSR : 클라이언트에서 직접 페이지를 렌더링 하는 방식이다.

SPA : 하나의 단일 페이지로 구성된 어플리케이션

<br/>

클라이언트가 첫 접속 시에 서버에 HTML, JS, CSS 리소스 파일을 서버에 요청하고, 모두다 받아온다.

이후 자바스크립트를 사용해 사용자의 상호작용에 따라 동적으로 페이지를 렌더링한다. - 서버와 통신 json

즉 이후에는 리로딩 없이, 필요한 부분만 서버와 json 으로 통신해 화면을 갱신한다.

하나의 HTML 에서 JS 로 동적으로 갱신되는 UI이다.

실제 페이지는 이동되지 않지만, 사용자는 이동되는 것처럼 보인다.

<br/>

클라이언트 측에 동적 dom 을 생성해서 동작하도록 한다.

<br/>

**렌더링 과정**

> 1\. 클라이언트가 url 에 접속하면, 서버에 http req 를 통해서 html 을 다운로드 한다.  
> 2\. 첫 로드 html 은 빈페이지 이지만, JS CSS 파일을 불러오는 링크가 포함되어있고, 링크를 통해 다운로드 한다.  
> 3\. 브라우저가 JS 코드를 실행하고, 이 안에서 React 가 구동되며 VirtualDOM 에 콘텐츠를 렌더링한다.  
> 4\. VirtualDOM이 구성되면, 이를 브라우저의 DOM(root 하위)에 붙인다.  
> 5\. 브라우저의 렌더링 트리 구성, 페인즈, 플로우 등의 과정을 거쳐 페이지를 그리고  
> 6\. 브라우저에 페이지가 나타난다.

<br/>

## 장단점

### 장점 : 첫 접속으로 페이지를 만든 이후에는, 

- 수시로 서버에 요청할 필요가 없으므로, 서버의 부담을 줄여준다.
- 동적으로 빠르게 렌더링 된다.
- 중복 리소스 요청이 없다.
- 데이터가 오프라인에서도 사용할 수 있어서 로컬테이터를 캐시(cache) 하기 유리하다.
- 컴포넌트 개발에 용이하다.

### 단점 : 첫 접속으로 초기 렌더링의 경우에,

- 서버에서 페이지를 만드는데 필요하는 모든 파일이 로드 되어야 페이지를 만들 수 있기때문에, 초기 렌더링이 매우 느리다.
  - 응용프로그램이 커질수록, 로드해야하는 파일 양이 증가해, 속도가 더 느려진다.
  - `webpack의 codesplitting`으로 해결하는 방법
- SEO 에 좋지 않다.
  - Search Engine Optimization : 검색엔진 최적화에 불리하다.
  - 검색엔진의 검색로봇이 크롤링 할때 어려움을 겪을 수 있다.
    - JS 기반 프로젝트이 경우, 자바스크립트 엔진이 내장된 검색엔진 크롤러가 아니면 수집불가
    - 자바스크립트 엔진 내장 크롤러 : 구글
    - 이외의 대부분은 내장되어있지않다.
  - heaer 메타데이터의 확정, CSR, SSR 혼합으로 해결하는 방법
  - 보안이슈 : 사용자 정보를 세션으로 관리 불가능, `localstroage` `Cookie`로 저장해야 하는데, XSS 공격에 취약하다

<br/>

# SSR Server-side Rendering - MPA Multiple Page Application

SSR : 서버에서 페이지를 렌더링해서 클라이언트에 전달하는 방식이다.

MPA : 여러개 페이지로 구성된 어플리케이션

<br/>

다른 페이지로 이동할 때마다, 서버에 새로운 페이지를 요청하고

즉시 서버연산을 통해 렌더링하고, 완성된 페이지 형태 HTML 을 응답한다.

<br/>

**렌더링 과정**

> 1\. 클라이언트가 url 에 접속하면, 서버에 http req 를 보낸다.  
> 2\. 서버는 요청 즉시 페이지를 만든다.  
> 2-1. Data Fetching (API call, Hydrate 등)을 미리 해서, 빈 페이지가 아닌, 초기 콘텐츠가 로딩된 페이지를 만든다.  
> 2-2. 브라우저는 이 HTML 페이지를 받아 페이지 DOM 에 그린다. - 페이지가 사용자에게 보인다.  
> 3\. 브라우저가 페이지를 그리면서, 동시에 태그 등을 통해 JS, CSS 파일 등을 로드한다.  
> 4\. 로딩된 JS 를 실행한다.  
> 5\. Interactive 한 페이지 구성이 완료된다.

<br/>

## 장단점

### 장점

- 초기 로딩 속도가 빠르다
- SEO 가능하다
  - Search Engine Optimization : 검색엔진 최적화
  - JS 엔진이 없는 검색로봇이 크롤링하기 적합하다.

### 단점

- 프론트와 백에 강하게 결합되어있다.
- 새로고침, 페이지 이동 시 화면이 깜빡거린다 (UX) > HTML `href` 태그를 통해 페이지 이동
- 부하 발생
  - 매 페이지마다 서버에 전송할 데이터를 저장해야 한다.
- JS 파일이 로드되기 전까지는 인터렉션 되지 않는다.

# 기술 적용

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAw6zR%2FbtsKuHCzdpP%2FjIGYoh1EulpIPiQMgcR5Ek%2Fimg.png" originWidth:3000 originHeight:2250 style:alignCenter >

## 1\. SSR 용 페이지 생성

React(SPA) 의 CSR 동작의 단점을 해결하기 위해, API 통신이 아닌, 실제 데이터를 포함한채로 렌더링되는 SSR 용 페이지를 별도를 만들어서 사용한다. 현재는 서버가 아닌 프론트엔드에서 Next.js, Nust.js, Node.js 를 이용해서 만들게 되고 백엔드와는 API 로만 통신된다.

즉, 서버측 코드로 프론트에서 페이지를 개발한다. 개발자원을 해결할 수 있다.

Next.js 의 경우는 SPA-CSR 환경에서 SEO 에 유리하기 위해 SSR 을 도입하였고, 바벨 웹팩의 툴로 환경설정 할 수있도록 지원해주어 React 개발자들이 많이 사용한다고 한다.

<br/>

**Next.js 작동 과정**

> 1\. SSR 방식  
> 1-1. 클라이언트가 처음 페이지에 접속했을때, 서버는 렌더링 될 HTML 값을 응답한다.  
> 2\. 브라우즈는 추가적으로 JS 번들을 다운로드 받아 실행한다.  
> 3\. CSR 방식  
> 3-1. 사용자가 다른 페이지로 이동할 때에는 서버가 아닌, 클라이언트에서 처리하여 이동한다.

<br/>

## 2\. ReactDOM - hydration 

전통적인 SSR과의 차이점은 hydration이라는 개념을 통해 미리 렌더링된 HTML에서 React가 동작 할 수 있도록 생명을 불어넣어 주는 것이다. 리액트 컴포넌트를 렌더링 하기 위해 빈 DOM 을 사용하는 대신, 모든 컴포넌트가 HTML로 렌더링 된 이미 빌드된 DOM 을 사용한다.

렌더링 과정에서 컴포넌트들을 렌더링하고, 이벤트 핸들러를 연결하는 JS 를 동작하게하는 이 과정을 `hydration` `하이드레이션`이라고 한다. 하이드레이션 후 앱은 이벤트가 동작할 수 있게된다. interactive

<br/>

**하이드레이션 Hydration 과정**

> 1\. SSR 로 서버에서 렌더링 된 html 전송  
> 2\. 클라이언트에서 JS 번들 다운로드  
> 3\. Hydration 과정  
> 3.1. JS 실행되면 서버 렌더링 된 html 을 기반으로 Virtual Dom 가상돔을 생성한다.  
> 3.2. 리액트는 기존 html 요서와 가상돔을 비교하여 어떤 부분이 동적으로 업데이트 되어야 하는지 판단한다.  
> 4\. 리액트는 각 html 요소에 대한 이벤트 핸들러를 연결한다. 인터랙티브  
> 5\. CSR 로 페이지 동적 업데이트

<br/>

### 기존 리액트 동작 방식

```
const root = document.querySelector("#root");
ReactDOM.render(<App name="Saeloun" />, root);

//전송 html
<html>
  <head></head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

<br/>

### SSR 방식으로 hydrate 설정된 동작 방식

```
// index.js
ReactDOM.hydrate(<App name="Saeloun"/>, document.getElementById('root'));

//server.js
import React from "react";
import ReactDOMServer from "react-dom/server";

app.use("/", (req, res, next) => {
  fs.readFile(path.resolve("./build/index.html"), "utf-8", (err, data) => {
    if (err) {
      console.log(err);
      return res.status(500).send("Some error happened");
    }

    return res.send(ReactDOMServer.renderToString(<App name="Saeloun" />)
    )
  });
});

//App.js
import React from "react";

function App(props) {
  return (
    <div>
      Hello {props.name}!
    </div>
  )
}

export default App;

//전송 html
<html>
  <head></head>
  <body>
    <div id="root">
      <h1>Hello Saeloun!</h1>
    </div>
  </body>
</html>
```

<br/>

# SSR 언제 어디에서 사용해야 하는가?

SPA 최근에 개발된 방식

SEO 가 얼마나 중요한가?

- public 데이터에만 필요하다
- 비즈니스 아이템에 SEO 가 필수적인가 관점
- 고객 타겟팅와 SEO 가 연관성을 지니는가

<br/>

## EX. 네이버 블로그 SSR

타겟 : 블로그 글을 읽는 네이버 메인 및 검색 사용자

비즈니스 상황 : 사용자는 빠르게 콘텐츠를 소비한다.

문제 :

- anuglar.js 프레임워크로 구현되어있음. - CSR 만 지원한다.
- 이로 인해 초기 로드시간이 길어 사용자 경험이 우려된다.

해결 :

- Next.js 로 SSR 구성하기를 React 공식 가이드에서 권장하고 있지만
- 네이버 블로그 검토 당시 Next.js 는 6.x ver 이었음. 9.x ver 부터 SSR 이 구성이 잘 되어있었음.
- Next.js 는 서비스 플로우 전체 담당하는 프레임워크, 의존도가 높아진다. - 뷰 전용 라이브러리를 사용해야 좋음

<br/>

# ✅ 배운점

SSR의 딥한 구현방법

Hydration

<br/>

#참고

[<공식문서> React 의 SSR 구현 시 Next.js 추천](https://ko.react.dev/learn/start-a-new-react-project#production-grade-react-frameworks)

[(번역) 리액트 앱(SSR)의 Hydration 이해하기](https://velog.io/@hyemin916/%EB%B2%88%EC%97%AD-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%95%B1SSR%EC%9D%98-Hydration-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

[10분 만에 React로 SSR 구현하기 (feat. Next.js 소스코드)](https://velog.io/@darren-kk/React%EB%A1%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-SSR-%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EA%B8%B0#nextjs%EB%8A%94)

[\[React\] 네이버 블로그의 Node.js기반 SSR 전환, CSR과 SSR방식을 자세하게 살펴보기](https://velog.io/@sunaaank/React-deep-dive)
