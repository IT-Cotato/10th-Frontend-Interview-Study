브라우저에서 웹 페이지의 스타일과 레이아웃은 동적으로 변화할 수 있다. 요소의 크기, 위치, 스타일 등을 변경하거나 브라우저 크기를 조절하는 경우, 페이지는 이를 감지하고 다시 계산하게 된다. 이런 과정에서 발생하는 것이 바로 Reflow와 Repaint이다. 이 두 작업은 웹 페이지 성능에 큰 영향을 미친다.

## 🖌️ Reflow

DOM 요소의 위치와 크기를 다시 계산하는 과정이다. 렌더 트리를 다시 생성하고, 레이아웃을 재계산하고, Reflow가 발생하면 Repaint도 발생하게 되므로 큰 비용이 발생한다.

상위 엘리먼트의 변경이 하위 엘리먼트에도 영향을 끼치며, 모든 관련 요소의 위치와 크기를 재계산한다.

### 발생 예시

- DOM 노드 추가/제거
- 요소의 위치 변경 (예: `position` , `top` , `left` 등)
- 크기 변경 (예: `width` , `height` , `margin` , `padding`등)
- 폰트 크기 변경
- 창 크기 변경
- 스크롤 위치 변경 (예: `scrollTop` , `scrollLeft` )
- 이미지 크기 변경
- CSS3 애니메이션과 트랜지션

!https://velog.velcdn.com/images/yongaricode/post/a02d0cfa-672a-41c3-b46a-6f02be66ff9a/image.png

```
속성과 메서드를 사용했다는 이유만으로도, 리플로우가 발생하는 것들
```

## 🎨 Repaint

Repaint는 레이아웃을 변경하지 않으면서도 화면에 시각적으로 보이는 요소만 변경하는 과정이다. Reflow가 발생하지 않지만, 화면에 요소를 다시 그려야하는 경우에 실행된다.

### 발생 예시

- 색상 변경 (예: `background-color` , `color` )
- 요소 가시성 변경 (`visibility` )
- 테두리 스타일 변경 (`outline` )

만약 Repaint는 Reflow에 비해 가벼운 작업이지만, 많은 양의 Repaint가 발생하면 성능에 부정적인 영향을 줄 수 있다.

---

정리하자면 **`Reflow`**는 위치와 크기를 계산하는 기하학적 변화를 반영한다면**, `Repaint`**는 기하학적 변화 외의 **눈으로 보여지는 부분**만 변경된다.

Reflow는 Repaint까지 발생시키는 비싼 작업이니까, 되도록 Reflow가 발생하지 않도록 주의해야한다.

## 💾 Reflow와 Repaint를 최소화하는 방법

### 1. **영향받는 노드 최소화하기**

- `position: fixed` 또는 `absolute`를 사용하여 상위 엘리먼트의 레이아웃 변경이 하위 엘리먼트에 영향을 미치지 않도록 한다.
- 레이아웃에 독립적인 요소로 처리되면, Reflow 발생 범위를 제한할 수 있다.

### 2. **숨겨진 엘리먼트 수정**

- **`display: none`**: Reflow와 Repaint를 모두 방지할 수 있다. 많은 엘리먼트를 변경할 때, 이 속성을 사용해 숨긴 후 수정하고 다시 보이게 하면 성능을 최적화할 수 있다.
- **`visibility: hidden`**: 보이지 않지만 요소가 공간을 차지하기 때문에 Repaint는 발생하지 않으나, Reflow는 발생한다.

### 3. **`transform`과 `opacity` 속성 사용하기**

- **`transform`**: 요소의 위치와 크기 변화를 레이아웃에 영향을 주지 않고 처리한다. `left`, `right` 대신 `transform`을 사용해 레이아웃 변경을 최소화한다.
- **`opacity`**: 시각적 변화만 주기 때문에 Reflow를 유발하지 않는다. `visibility` 대신 사용하여 성능을 최적화할 수 있다.

### 4. **애니메이션 최적화**

- **`requestAnimationFrame()`**: 브라우저의 프레임 속도(60fps)에 맞춰 애니메이션을 처리해 자연스럽게 실행된다. 페이지가 비활성 상태일 때 렌더링을 중단하여 리소스를 절약할 수 있다.
- **`setTimeout()`**: 이벤트 루프에 의해 딜레이가 발생하고, 백그라운드 상태에서도 계속 실행되므로 성능에 좋지 않은 영향을 미칠 수 있다.

### 5. **인라인 스타일 피하기**

- 인라인 스타일은 HTML 파싱 시 레이아웃에 영향을 주어 Reflow를 유발한다. 또한 코드의 가독성과 유지 보수성을 저하시킬 수 있어, 가급적 CSS 파일로 스타일을 분리하는 것이 좋다.

### 6. **Table 레이아웃 피하기**

- `<table>`은 한 번에 전체 레이아웃을 계산하고 그리므로, 작은 변화에도 전체 테이블에서 Reflow가 발생한다. 레이아웃 목적으로 테이블을 사용하는 것은 피해야 한다.

### 7. **DOM 사용 최소화**

- DOM에 빈번하게 접근하거나 수정하는 것은 성능에 부담을 준다. **DOM Fragment**를 사용하여 여러 요소를 한 번에 삽입하고, DOM 접근을 최소화해 Reflow와 Repaint 발생을 줄인다.
- `DocumentFragment`는 메모리 상의 임시 컨테이너이다. 이는 DOM 트리의 일부가 아니기 때문에, `DocumentFragment`에 여러 엘리먼트를 추가하거나 변경해도 실제 DOM에는 영향을 미치지 않는다.
  따라서, **DOM에 추가될 때까지 Reflow가 발생하지 않는다.** 여러 요소를 `DocumentFragment`에 추가하고, 마지막에 한 번만 DOM에 삽입하면, 중간에 발생할 수 있는 여러 Reflow를 한 번으로 줄일 수 있다.

### 8. **캐시 활용하기**

- 브라우저는 레이아웃 변경을 큐에 저장했다가 한 번에 처리해 Reflow를 최소화한다. 그러나 `offset`, `scrollTop` 등의 계산된 스타일 정보를 요청하면 즉시 큐를 비우고 모든 변경사항을 반영하므로, 이러한 속성의 접근을 최소화해야 한다.

## Reference

> https://velog.io/@ddowoo/JavaScript-Document-Fragment의-기본-개념과-성능-최적화-활용-방법
> https://devowen.com/463
> https://velog.io/@leitmotif/놓치기-쉬운-reflow-repaint
> https://enjoydev.life/blog/frontend/3-reflow-repaint
> https://ekimnida.tistory.com/45
> https://velog.io/@young_pallete/Reflow-Repaint을-알아보자
