## CRP

- 주요 렌더링 경로
- 웹 페이지를 화면에 표시하기 위해 거치는 과정

### CRP 단계

1. HTML 파싱 및 DOM 트리 구축
    - DOM: 웹 페이지를 이루는 태그들을 자바스크립트가 이용할 수 있게끔 브라우저가 트리구조로 만든 객체 모델
2. CSS 파싱 및 CSSOM 트리 구축
3. JavaScript 실행
    - **HTML 렌더링 중에 JavaScript가 실행되면 렌더링이 멈추는 이유**
        
        DOM 생성을 임시 중단하고, 자바스크립트 코드를 파싱하기 위해 자바스크립트 엔진에 제어권을 넘김
        
4. DOM 및 CSSOM을 결합하여 렌더 트리 구축
5. 레이아웃 단계(Reflow)
    - DOM, CSS 변경
    - CSS의 모든 애니메이션 프레임
    - offsetWidth 와 offsetHeight 의 사용
6. 페인트 단계
    - 가시성 변경 (opacity, background-color, visibility, outline)
    - Reflow가 발생한 뒤에 항상 실행됨

## defer & async
### defer
- script에 부여할 수 있는 속성
- 렌더 트리가 구축된 이후 실행됨
- script 파일을 비동기적으로 다운로드함

### async
- script에 부여할 수 있는 속성
- 다운로드가 완료되는 즉시 실행됨
- script 파일을 비동기적으로 다운로드함