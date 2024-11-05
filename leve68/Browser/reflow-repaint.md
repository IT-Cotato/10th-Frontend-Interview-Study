## Reflow

DOM 요소의 위치와 크기를 다시 계산하는 과정으로 렌더 트리를 다시 생성

상위 엘리먼트의 변경이 하위 엘리먼트에도 영향을 끼치며, 모든 관련 요소의 위치와 크기를 재계산

페인트 레이어 분리 ⇒ 리렌더링 영향 최소화

- DOM 노드 추가/제거
- 스크롤 위치 변경 (예: `scrollTop` , `scrollLeft` )
- 이미지 크기 변경
- CSS3 애니메이션과 트랜지션

position, width, height, left, top, right, bottom, margin, padding, border, border-width, clear, display, float, font-family, font-size, font-weight, line-height, min-height, overflow, text-align, vertical-align, white-space

## Repaint

Repaint는 레이아웃을 변경하지 않으면서도 화면에 시각적으로 보이는 요소만 변경하는 과정

background, background-image, background-position, background-repeat, background-size, border-radius, border-style, box-shadow, color, line-style, outline outline-color, outline-style, outline-width, text-decoration, visibility

만약 Repaint는 Reflow에 비해 가벼운 작업이지만, 많은 양의 Repaint가 발생하면 성능에 부정적인 영향을 줄 수 있다.

### 애니메이션

리플로우와 리페인트를 일으키는 width,height,color,background-color 등의 속성이 아닌 `transform`,`opacity` 속성을 이용한 애니메이션 성능이 더 좋음 ⇒ 컴포지트 속성

컴포지트 속성은 기본값이면 리플로우 발생

Paint layer 생성 규칙 Stacking context를 생성하는 값이 기본값이 아닌 경우 생성함

**페인트 레이어 생성 규칙: Stacking context를 생성하는 값이 기본값이 아닌 경우 생성함**

- **페인트(Paint) 레이어**: 브라우저는 웹 페이지를 렌더링할 때, 화면에 그려질 내용을 여러 레이어로 나누어 처리
  성능 최적화를 위해 각 레이어가 독립적으로 처리되기 때문에 전체 페이지를 다시 그리지 않고도 부분적인 업데이트가 가능
- **Stacking context**: 특정 CSS 속성 값에 의해 요소는 **스태킹 컨텍스트**를 형성
  예를 들어, **`position: relative`**, **`z-index`**, **`opacity`**, **`transform`** 등 일부 속성들이 기본값이 아닌 경우, 새로운 스태킹 컨텍스트가 만들어지며 해당 요소는 새로운 페인트 레이어에 그려짐
