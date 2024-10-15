# Reflow와 Repaint

## Reflow와 Repaint

![browser_rendering_chart](/MinJaeSon/Images/browser_rendering_chart.png)
![alt text](image.png)

브라우저의 렌더링 과정에서 렌더 트리가 생성된 후, **뷰포트를 기반으로 요소(노드)들의 위치, 크기 등을 계산하는 과정을 Layout 단계**, 이를 바탕으로 **화면에 요소들을 픽셀화하여 그리는 과정을 Paint 단계**라고 한다. <br/><small>([브라우저의 렌더링 과정에 대해 더 자세히 알고 싶다면 여기를 참고](https://github.com/IT-Cotato/10th-Frontend-Interview-Study/blob/develop/MinJaeSon/Browser/browser-rendering.md))</small>

Reflow는 Layout과, <br/>
Repaint는 Paint와 연관되어 있는데, <br/>

쉽게 말해 **Reflow는 레이아웃이 변경되었을 때, 각 요소들의 위치나 크기 등을 다시 계산하는 과정**이고, (변경 시 영향을 받은 모든 노드의 수치를 계산 -> 렌더트리 재생성)<br/>
**Repaint는 Reflow가 일어나고 화면을 다시 그리는 과정**을 말한다.

<br/><br/>

## Reflow와 Repaint가 발생하는 시점

### Reflow 발생 예시

- 윈도우 리사이징 (뷰포트 변화 -> Global Layout에 영향)
- 폰트의 변화 (`height` 계산에 영향을 주므로 Global Layout에 영향)
- 스타일 추가 또는 제거 (레이아웃 변경을 유발)
- 내용 변화 (ex. input 박스에 텍스트 입력 시)
- CSS Pseudo Class 사용 (ex. `:hover` -> 마우스호버 시 변화)
- JS를 통한 DOM의 동적 변화
- 스타일 속성의 동적 변화
- 클래스 속성의 동적 변화

<br/>

CSS 속성으로 예시를 들어보자면

`position` `width` `right` `margin` `padding` `clear` `display` `float` `font-family` `line-height` `min-height` `overflow` `text-align` `white-space` ...
이렇게 엄청나게 많다.

요소의 크기나 위치가 변경될 수 있는 것들은 일단 거의 해당된다고 보면 될 것 같다.

혹은 `offsetTop`, `clientWidth`와 같이 위치값을 읽어야 하는 경우에도 연산이 필요하기에 reflow가 발생한다. `scrollTo()`나 `focus()`도 마찬가지.

관련 속성들을 더 알고 싶다면 [여기](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)에 자세히 나와있으니 참고하자.

<br/>

### Repaint 발생 예시

- 리플로우 발생 직후
- 요소의 스타일(ex. 색상, 배경색 등)의 변화
- `visiblity: hidden` <-> `visible` 변경 시

CSS 속성 예시는

`background` `color` `text-decoration` `outline` `visiblity` `opacity` ...
등이 있다.

<br/>

> 💡 **`visiblity` 속성처럼 `display: none`도 Repaint를 유발하는 속성일까?**
>
> 공간을 차지하지만 화면에만 보이지 않는 `visiblity`와 달리 `display`는 공간을 차지하지조차 않기 때문에, `display: none`가 적용된 요소는 DOM에서 제외되면서 주변 요소에도 영향을 미쳐 reflow + repaint가 발생하게 된다.

<br/>

> 💡 **무조건 Reflow가 발생해야 Repaint가 발생될까?**
>
> 🙅🏻‍♂️. `background-color`, `visiblity`와 같이 레이아웃에 영향을 주지 않는 경우는 Reflow가 발생하지 않고도 Repaint만 발생할 수 있다.

<br/>

> 📕 각 CSS 속성 변화에 따라 reflow, repaint에 어떤 영향을 주는지 하나 하나 알고 싶다면? -> [참고해볼만한 자료](https://csstriggers.com/)

<br/><br/>

## Reflow와 Repaint를 최적화하는 방법

### - 애니메이션 요소의 `position`을 `absolute` 또는 `fixed`로 하기

`animation`, `transition` 등은 많은 reflow 연산을 발생시키기도 하는데, `position`을 `absolute` 또는 `fixed`로 하면 주변 요소들에 영향을 주지 않기 때문에 해당 노드만 reflow가 발생한다.

<br/>

반대로, `position: relative`와 같은 속성은 주의하자.

### - DOM 속성 변경 코드 모으기

```js
// BAD

const el1 = document.querySelector(".target-first");
el1.style.width = "10px";

const el2 = document.querySelector(".target-second");
el2.style.width = "10px";

const el3 = document.querySelector(".target-third");
el3.style.width = "10px";
```

이 같은 코드는 리플로우를 3번 유발하는데, 아래와 같이 코드 순서만 변경해줘도 리플로우 횟수를 1번으로 줄일 수 있다.

```js
// GOOD

const el1 = document.querySelector(".target-first");
const el2 = document.querySelector(".target-second");
const el3 = document.querySelector(".target-third");

// DOM의 스타일을 변경하는 코드를 한 곳에 모아둔다
el1.style.width = "10px";
el2.style.width = "10px";
el3.style.width = "10px";
```

<br/>

> 💡 브라우저는 리플로우를 처리할 때, 변경할 요소를 큐에 저장하였다가 일정 시간이 지나거나 변경 작업이 쌓였을 때 리플로우를 실행한다.

### - 인라인 스타일보다는 CSS 클래스 사용하기
인라인 스타일이 수차례의 리플로우를 발생시킨다. 만약 DOM의 여러 스타일을 변경해야 한다면, 스타일을 CSS 클래스로 정의해두고 한 번에 변경해보자.
```js
// BAD
el.style.width = "10px";
el.style.height = "10px";
el.style.borderRadius = "5px";
el.style.backgroundColor = "red";
el.style.left = "20px";

// GOOD
<body>
  <script>
    el.className = 'small';
  </script>
</body>

<style>
.small {
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: red;
  left: 20px;
}
</style>
```

### - 리플로우 유발 함수의 호출을 제한하기
리플로우를 유발하는 함수나 속성을 매번 호출하지 않고 변수에 저장하여 사용할 수 있다.

```js
// BAD
for (let i = 0; i< 10; i++) {
  el.style.width = target.offsetWidth + 10; // 반복문이 돌 때 매번 호출
}

// GOOD
const { offsetWidth } = target;  // 한 번 호출해서 변수에 저장
for (let i = 0; i< 10; i++) {
  el.style.width = offsetWidth + 10;
}
```

### - CSS 하위 선택자 줄이기
CSS 선택자가 구체적일수록 브라우저가 찾는 속도가 더 느리다. 아래와 같은 예제에서, `.btn`이 부모 객체인 `li`를 가지고 있는지 확인하기 위해 DOM을 거슬러 올라가기 때문이다. 

```css
/* BAD */
.container .list li .btn {
  background-color: red;
}

/* GOOD */
.list .btn {
  background-color: red;
}
```

### - `<table>` 사용 자제하기
테이블은 내부 컨텐츠가 모두 로딩된 후에 그려지고 작은 변화에도 테이블 전체 노드의 리플로우가 발생될 수 있기 때문에 사용을 자제하거나, 쓰게 된다면 `table-layout: fixed`로 테이블의 크기를 고정하는 것이 좋다.

### - 프레임 줄이기
애니메이션 또는 트랜지션 효과를 사용할 때는 성능을 고려하여 적절히 트레이드오프하는 것이 좋다. (퀄리티와 퍼포먼스를 타협)

### - `display: none` 사용하기
`display: none`이 적용된 요소를 렌더트리에서 제외된다. 따라서 요소의 여러 스타일이 수정되어야 하는 경우, `display: none`으로 설정한 뒤 스타일을 변경하고 다시 `display: block`을 해주면 리플로우, 리페인트에 드는 비용을 절감할 수 있다.

```js
div.style.display = "none";   // 렌더트리에서 제외

// 스타일 변경

div.style.display = "block";  // 렌더트리에 다시 추가
```

### - Virtual DOM 사용하기
DOM에 변경사항이 있을 때, 가상 DOM이 실제 DOM과 비교하여 변경된 부분만 실제 DOM에 반영한다. React에서 이러한 Virtual DOM을 사용한다.


<br/><br/>

## 💡 더 알아보기 - Composite Layer

### Pixel Pipeline
![pixel pipeline](/MinJaeSon/Images/pixel_pipeline.png)
브라우저가 렌더트리를 화면에 그릴 때, layout과 paint 외에도 **composite**이라는 단계가 존재한다.

이 과정은 **스타일 지정 방식에 따라 화면에 나타날 요소들이 레이어로 분류되어 계층이 결정되고, 레이어들을 순서에 맞게 화면에 그리는 합성 과정**이다.

Composite Layer는 **GPU**의 메모리를 통해 작업을 처리하기 때문에 **layout 및 paint 과정이 없이 화면을 구성할 수 있다.**

<br/>

Composite을 트리거하는 속성은 대표적으로

`transform`, `opacity`(1보다 작아야 함. 1 미만일 때만 layer를 트리거하기 때문) 가 있는데,

이 속성들을 사용하여 애니메이션을 구현 시 성능이 가장 좋은 버전의 pixel pipeline을 구성할 수 있다. (Layout과 Paint 과정이 모두 생략됨)

![pixel_pipeline_when_use_transform_and_opacity](/MinJaeSon/Images/pixel_pipeline_when_use_transform_and_opacity.png)



<br/><br/><br/>
---
참고 : <br/>
[Reflow, Repaint을 알아보자!](https://velog.io/@young_pallete/Reflow-Repaint%EC%9D%84-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90) <br/>
[리플로우, 리페인트와 브라우저 렌더링 알아보기](https://mong-blog.tistory.com/entry/%EB%A6%AC%ED%94%8C%EB%A1%9C%EC%9A%B0-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8%EC%99%80-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0) <br/>
[놓치기 쉬운 Reflow, Repaint](https://velog.io/@leitmotif/%EB%86%93%EC%B9%98%EA%B8%B0-%EC%89%AC%EC%9A%B4-reflow-repaint#reflow%EB%A7%8C-%EC%9D%BC%EC%96%B4%EB%82%98%EA%B3%A0-repaint%EB%A7%8C-%EC%9D%BC%EC%96%B4%EB%82%A0-%EC%88%98-%EC%9E%88%EB%82%98%EC%9A%94) <br/>
[[JavaScript] 렌더링 최적화 (Reflow와 Repaint)](https://seokzin.tistory.com/entry/JavaScript-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%B5%9C%EC%A0%81%ED%99%94-Reflow%EC%99%80-Repaint) <br/>
[Composite Layer(feat. css, js 애니메이션)](https://velog.io/@nala723/CSS-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98-vs-JS-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98) <br/>
[중요 렌더링 경로 - 웹 성능 - MDN Web Docs](https://developer.mozilla.org/ko/docs/Web/Performance/Critical_rendering_path) <br/>
[Mastering Web Performance: Understanding the Pixel Pipeline](https://dev.to/juanjefry23/css-3ple) <br/>
[Stick to Compositor-Only Properties and Manage Layer Count](https://web.dev/articles/stick-to-compositor-only-properties-and-manage-layer-count) <br/>
[Composite Layer(feat. css, js 애니메이션)](https://velog.io/@nala723/CSS-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98-vs-JS-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98#%EF%B8%8F-layout%EC%9D%84-%EC%9C%A0%EB%B0%9C%ED%95%98%EB%8A%94-%EC%8A%A4%ED%83%80%EC%9D%BC%EB%A9%94%EC%86%8C%EB%93%9C)