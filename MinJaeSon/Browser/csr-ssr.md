# CSR과 SSR

## MPA와 SPA

### MPA(Muliple Page Application)

새로운 페이지를 요청할 때마다 서버에 정적 리소스를 요청

ex.JSP, PHP

### SPA(Single Page Application)

모든 정적 리소스를 최초 한 번에 다운로드

ex. React, Vue, Next, Nuxt

### 🤷🏻‍♂️ MPA는 SSR, SPA는 CSR?

일반적으로 MPA는 SSR, SPA는 CSR 방식을 많이 사용하나, 고정된 규칙은 아니다.
예외가 되는 사례들을 살펴보자.

- SSR을 사용하는 SPA

  Next.js는 기본적으로 SPA임에도 불구하고, SSR과 CSR을 혼합한 하이브리드 렌더링 방식을 사용한다.

- CSR을 사용하는 MPA

  일부 MPA는 초기 HTML을 서버에서 제공하고, 나머지 상호작용을 위해 JavaScript를 통해 데이터를 가져와서 클라이언트에서 렌더링하는 식으로 구현되기도 한다.

- SSG와 CSR의 혼합

  Gatsby나 Next.js의 정적 사이트 생성 기능을 사용하면, 빌드 시 HTML을 생성해 빠르게 제공하고, 이후에는 CSR로 동작하는 SPA를 만들 수 있다.

- SSR과 CSR의 혼합 MPA

  페이지 로딩 시 서버에서 데이터를 제공하되, 일부 기능은 클라이언트에서 JavaScript를 통해 동적으로 작동하도록 CSR 방식으로 처리하는 경우도 있다.

<br/>

💁🏻‍♀️ **결론 :** 각 렌더링 방식의 특징을 고려하여 필요에 따라 적절히 선택하여 활용하는 것이 중요하다.

<br/>

## CSR(Client Side Rendering)

서버가 클라이언트에게 HTML 파일과 JS로 접근할 수 있는 링크를 보내주고, 클라이언트인 브라우저가 해당 파일들을 렌더링 하는 방식

**장점**

- 사용자 경험이 좋다 (새로고침 없이 화면 전환이 부드럽게 되어 화면 깜빡임이 없음)
- 서버 자원 활용이 적다 (index.html 파일만 서버에 요청)

**단점**

- 페이지의 초기 로딩 속도가 비교적 느리다 (초기에 빈 html 파일만 보여지기 때문)
- SEO(Search Engine Optimization)에 좋지 못하다
- 보안에 취약하다

<br/>

## SSR(Server Side Rendering)

서버에서 사용자에게 보여줄 페이지를 모두 구성하여 사용자에게 페이지를 보여주는 방식

**장점**

- 페이지의 초기 로딩 속도가 비교적 빠르다
- SEO(Search Engine Optimization)에 좋다
- 보안에 유리하다

**단점**

- 사용자 경험이 좋지 못하다
- 많은 서버 자원을 활용한다

<br/>

## ISR과 SSG

SSG와 ISR은 주로 초기 로딩 성능을 극대화하거나 자주 변하지 않는 콘텐츠를 위해 활용된다.

### SSG(Static Site Generation)

**서버에서 정적인 HTML을 미리 생성하는 방식** <br/>
서버에서 HTML 파일을 받아온다는 측면에서는 SSR과 유사하지만 클라이언트가 요청하는 시점이 아니라 **빌드 시에 HTML을 미리 생성**한다는 차이점이 있다.

> 💡 **어떤 경우에 활용될까?**
>
> 콘텐츠가 자주 변경되지 않고, 모든 사용자에게 동일한 정보를 제공하는 경우 (ex. 블로그, 문서 페이지 등)

**장점**

- 페이지의 초기 로딩 속도가 비교적 빠르다
- SEO(Search Engine Optimization)에 좋다
- 보안에 유리하다

**단점**

- 내용이 정적이다
- 실시간 데이터가 아니다
- 사용자별 정보 제공이 어렵다

### ISR(Incremetal Static Regeneration)

**서버에서 주기적으로 HTML을 생성해두고 내려주는 방식** <br/>
처음 빌드 시점에 만들어두고, 설정한 주기마다 다시 렌더링하여 HTML을 생성한다.

> 💡 **어떤 경우에 활용될까?**
>
> 일정 시간 간격으로 업데이트가 필요한 페이지나 약간의 실시간성이 필요한 콘텐츠 (ex. 뉴스, 상품 목록 페이지 등)

**장점**

- 페이지의 초기 로딩 속도가 비교적 빠르다
- SEO(Search Engine Optimization)에 좋다
- 보안에 유리하다

**단점**

- 실시간 데이터가 아니다
- 사용자별 정보 제공이 어렵다

<br/>
<br/>
<br/>

참고 :

https://github.com/IT-Cotato/8th-education/blob/develop/Week13/13%EC%A3%BC%EC%B0%A8_CSR%EA%B3%BC_SSR.pdf <br/>
https://medium.com/@zero86/next-js-csr-ssr-isr-ssg-%ED%95%98%EC%9D%B4%EB%B8%8C%EB%A6%AC%EB%93%9C-hybrid-%EB%A0%8C%EB%8D%94%EB%A7%81-%ED%95%98%EC%9D%B4%EB%93%9C%EB%A0%88%EC%9D%B4%EC%85%98-hydration-e2f6a487fe95