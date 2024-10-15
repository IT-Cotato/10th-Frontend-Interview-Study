### < 브라우저의 렌더링 단계 >

> 1\. HTML 파싱 → DOM 트리 구조 구축  
> 2\. CSS 파싱 → CSSOM 트리 구조 구축  
> 3\. JS 실행 \*HTML 중간에 스크립트가 있다면 HTML 파싱이 중단된다.\*  
> 4\. DOM과 CSSOM을 조합하여 렌더트리 구축  
> **5\. Layout 단계: 뷰포트를 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기를 계산한다. (Reflow)**  
> **6\. Paint 단계: 계산한 위치/크기를 기반으로 화면에 그린다. (Repaint)**

[\[참고\] 브라우저의 렌더링 원리](https://ansui218.tistory.com/116)

### < Reflow와 Repaint 관계 >

브라우저의 렌더링 단계를 보면 Reflow는 Repaint를 선행한다.

→ 즉, Repaint만 일어날 수 있지만 Reflow는 Repaint를 동반한다.

---

### < Reflow >

- 생성된 DOM 노드의 레이아웃 수치(너비, 높이, 위치 등) 변경 시 영향받은 모든 노드(자신, 자식, 부모, 조상)의 수치를 다시 계산하여(Recalculate) 렌더트리를 재생성하는 과정

### < Repaint >

- Reflow 과정이 끝난 후 재생성된 렌더 트리를 다시 그리는 과정

- 레이아웃에 영향을 주지않지만 눈에 보이는 요소들(background-color, color, visibility,..)이 될 때 발생한다.

- Reflow 보다는 부하가 크지 않다.

- 전체 픽셀을 다시 계산하는 Reflow와 달리, Repaint는 이미 계산된 값을 활용하기 때문에 비용이 적게든다.

---

### < 실행 시점 >

### Reflow

- 페이지 초기 렌더링 (최초 Layout 과정)

- DOM 노드를 추가, 제거, 변경하는 경우

- DOM 요소의 위치 변경, 크기 변경 (margin, padding, border, width, height...)

  ```
  // DOM에 노드 추가 → Reflow 발생
  const newDiv = document.createElement('div');
  document.body.appendChild(newDiv);

  // 요소의 크기 변경 → Reflow 발생
  const element = document.getElementById('myElement');
  element.style.width = '200px';
  ```

- CSS 스타일 추가, 제거, 변경하는 경우

- offset, scrollTop, scrollLeft와 같은 계산된 스타일 정보 요청

- 이미지  크기 변경

- CSS3 애니메이션과 트랜지션, 애니메이션의 모든 프레임에서 발생

- offsetWidth와 offsetHeight의 사용

  **\*offsetWidth, offsetHeight:** margin을 제외한, padding값, border값까지 계산한 값을 가져온다

  → offsetWidth와 offsetHeight 속성을 읽을 시 초기 리플로우가 트리거되어 수치가 계산된다.

- 유저 인터랙션으로 발생하는 hover효과, 필드에 텍스트 입력, 창 크기 조정, 글꼴 크기 변경, 스타일시트 또는 글꼴 전환 등을 활성화하는 경우

### Repaint

- 레이아웃에는 영향을 미치지 않지만 가시성이 변경되는 순간 (opacity, background-color, visibility, outline)

  → Repaint만 발생

- Reflow가 실행 된 순간 뒤에 실행된다.

---

### < Reflow 최소화 >

- 불필요한 노드는 **display: none**으로 변경한다.

  visibility: invisible은 Layout 공간을 차지하기 때문에 Reflow의 대상이 된다.

  하지만 display: none은 Layout 공간을 차지하지 않아 Render Tree에서 아예 제외되므로 렌더링 성능을 향상할 수 있다.

- 클래스 변경을 통해 스타일을 변경할 경우, 최대한 **말단의 노드의 클래스를 변경한다.**

- **CSS에서 하위선택자를 줄인다.**

  하위 선택자가 많아지면 CSSOM Tree의 깊이가 깊어지고 Render Tree를 만드는 시간이 늘어나게 된다.

  또한 Reflow마다 부모 선택자를 매칭 하는 연산이 길어지는 만큼 성능에 영향을 미치게 된다.

  ```
  .container .list li .btn {
  background-color: red;
  }

  // 하위선택자를 줄여 최적화
  .list .btn {
  background-color: red;
  }
  ```

- **인라인 스타일의 사용을 지양한다.**

  인라인 스타일은 HTML이 파싱 될 때 레이아웃에 영향을 주어 추가적인 Reflow를 발생시킨다.

  물론 유지보수 및 가독성 측면에서도 인라인 스타일을 지양하는 것이 좋기도 하다.

  **\*인라인:** 한 줄짜리 짤막한 스타일, 태그 안에 직접 지정하여 사용, HTML과 섞어서 사용

  **\*인터널:** HTML 파일 안에 별도 영역으로 스타일 정의

  **\*익스터널:** CSS파일을 별도의 페이지로 만들어서 사용

- 애니메이션이 들어간 엘리먼트는 **position:fixed 또는 position: absolute**로 지정한다.

  JS+CSS를 활용한 에니메이션 효과는 해당 프레임에 따라 무수히 많은 Reflow 비용이 발생하게 된다.

  하지만 position 속성을 "fixed" 또는 "absoute"로 값을 주면 지정된 노드는 전체 노드에서 분리된다.

  ( 다른 엘리먼트의 레이아웃에 영향을 미치지 않는다. )

  → 즉, 전체 노드에 걸쳐 Reflow 비용이 들지 않으며, 해당 노드의 Repaint 비용만 들어가게 된다.

- 레이아웃을 위한 **table은 피하거나** **table-layout: fixed**를 사용한다.

  테이블로 구성된 페이지 레이아웃은 점진적(progressive) 페이지 렌더링이 적용되지 않으며, 모두 로드되고 계산(Recalculate)된 후에야 화면에 나타나므로 피하는 게 좋다.

  그러나 만약 사용한다면 해당 테이블에 table-layout:fixed 속성을 주는 것이 디폴트값인 auto에 비해 성능면에서 더 좋다.

  **\*progressive rendering:** 중요한 컨텐츠만 먼저 렌더링 → 클라이언트로 스트리밍 → 중요하지 않은 컨텐츠 렌더링 → 스트리밍

- CSS에서 **JS식 사용하지 않는다**.

  문서 전체 또는 문서 중 일부가 Reflow될 때마다 표현식이 다시 계산되므로 JS를 사용하지 않도록 한다.

- JS를 통해 스타일을 변경할 경우 **.cssText를 사용**하거나 **클래스를 활용(\*\***DOM과 스타일 변경을 하나로 묶음)\*\* 한다.

  ```
  element.style.width = '100px';
  element.style.height = '100px';
  element.style.backgroundColor = 'blue';

  //cssText 이용하여 최적화
  element.style.cssText = 'width: 100px; height: 100px; background-color: blue;';

  // 클래스 이용하여 최적화
  element.classList.add('new-style');

  // css파일에서 new-style 정의
  .new-style {
  width: 200px;
  height: 200px;
  background-color: blue;
  border-radius: 10px;
  }
  ```

- 캐쉬를 활용하여 수치에 대한 **스타일 정보를 변수에 저장**하여 정보 요청 횟수를 줄인다.

  ```
  const element = document.getElementById('myElement');

  // 값을 변수에 저장 (Reflow O)
  const width = element.offsetWidth;
  const height = element.offsetHeight;

  // 변수에 저장된 값을 재사용 (Reflow X)
  console.log(width);
  console.log(height);
  ```

---

### < 참고자료 >

**[https://seokzin.tistory.com/entry/JavaScript-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%B5%9C%EC%A0%81%ED%99%94-Reflow%EC%99%80-Repaint](https://seokzin.tistory.com/entry/JavaScript-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%B5%9C%EC%A0%81%ED%99%94-Reflow%EC%99%80-Repaint)**

**[https://webclub.tistory.com/346](https://webclub.tistory.com/346)**

[https://k0102575.github.io/articles/2020-11/reflow-repaint](https://k0102575.github.io/articles/2020-11/reflow-repaint)

[https://likelion-kgu.tistory.com/81](https://likelion-kgu.tistory.com/81)
