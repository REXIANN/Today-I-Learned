## CSS는 스타일을 정의한다

HTML은 구조화된 문서를 작성한다. 먼저 유명한 짤방 감상....

CSS는 우리가 상상하는 대로 동작하지 않을 가능성이 크다.

## CSS의 기본 구조

```css
h1 { /* 셀렉터 */
	color: blue; /* 선언 과 구현 */
	font-size: 15px;
}
```

## CSS의 정의 방법

- 인라인: 해당 캐그 안에 직접 style 속성을 사용하여 집어넣음
- 내부 참조: HTML의 파일 안의 `<style>` 태그 내에 지정
- 외부 참조: 외부 CSS파일을 `<link>`를 통해 불러오기

가장 많이 사용되는 프로퍼티(속성)값은 font-size, color, margin-top, margin-left, width, height 등등이 있다.

## CSS 선택자

HTML문서에서 특정한 요소를 선택하여 스타일링 하기 위해서는 반드시 선택자라는 개념이 피요하다.

- 기초 선택자
- 고급 선택자
    - 자손 선택자, 직계 자손 선택자
    - 형제, 인접형제 선택자, 전체 선택자
- 의사 클래스(pseudo class)
    - 링크, 동적 의사 클래스
    - 구조적 의사 클래스
    - 기타 의사 클래스, 의사 엘리먼트, 속성 선택자


`#sect1 > ul > li:nth-child(1)` 의 의미: sect1 이라는 id를 가진 블럭 안의 ul 태그 안에 있는 li 태그 요소들 중 첫번째 태그를 잡음

⚠️ 문서에서 id는 한번만 사용해야 한다. (유니크한 값임을 보장) 물론 두번 사용해도 문제 없이 렌더링이 되겠지만 약속상 하지 않는다.

## CSS 상속

CSS는 상속을 통해 부모요소의 속성을 ~~모두~~  자식에게 상속한다.

- 속성(Property) 중에는 상속이 되는 것과 되지 않는 것들이 있다.
- 상속되는것 → Text 관련 요소(font, color, text-align), opacity, visibility 등
- 상속안되는것 → Box model 관련 요소(width, height, margin, padding, border, box-size, display), position 관련 요소(position, to/right/bottom/left, z-index) 등

## CSS 적용 우선순위 (cascading order)

- 중요도(Importance)
    - !important → ‼️ 사용시 주의가 필요하다!
- 우선순위(Specificity)
    - 인라인 // id 선택자 // class 선택자, 속성 선택자, pseudo-class // 요소 선택자, pseudo-element
- 소스 순서

CSS 파일

```css
h3 { color: violet !important }
p { color: green; }
.blue { color: blue; }
.skyblue { color: skyblue; }
#red { color: red; }
```

HTML 파일

```html
<p>1</p>
<p class="blue">2</p>
<p class="blue skyblue">3</p>
<p class="skyblue blue">4</p>
<p id="red" class="blue">5</p>
<h3 id="red" class="blue">6</h3>
<p id="red" class="blue" style="color: yellow;">7</p>
<h3 id="red" class="blue" style="color: yellow;">8</h3>
```

- 3, 4가 둘다 하늘색인 이유: HTML 파일 내에서 선언되는 순서는 중요하지 않고, css파일 내에서 선언되는 순서가 중요하다. 하늘색이 더 나중에 선언되어 있으므로 우선순위를 가지게 된다. 렌더링시 HTML을 먼저 읽고 CSS를 읽으면서 해당 클래스에 해당하는 HTML 요소들에  스타일을 적용시키기 때문이다.

## CSS 크기 단위

- px(픽셀)
- %
- em: 배수 단위, 요소에 지정된 사이즈에 상대적인 사이즈를 가짐
- rem: 최상위 요소(html)의 사이즈를 기준으로 배수 단위를 가짐(root em)
- viewport 기준 단위 ⇒ vw, vh, vmin, vmax

## CSS 색상 단위

- HEX(00 ~ ff) → #ffffff;
- RGB(0 ~ 255) → rgb(255, 255, 211);
- RGBA(0 ~ 255, 0 ~ 1) → rgba(0, 12, 12, 0.5);

## CSS 문서 표현

- 텍스트
    - 변형 서체
    - 자간, 단어간격, 행간, 들여쓰기
    - 기타 꾸미기
- 컬러, 배경(background-image, background-color)
- 목록 꾸미기
- 표 꾸미기

## CSS Box model(박스 모델)

### box-sizing

기본적으로 모든 요소의 box-sizing은 content-box 이다. 이는 padding을 제외한 순수 contents 영역만을 box로 지정한다는 것을 의미한다.

그러나 우리가  일반적으로 영역을 볼 때는 border까지의 너비를 100px로 보길 원한다. 따라서 이 경우 box-sizing을 border-box로 해준다. border까지를 하나의 박스로 보겠다는 의미이다.

### 마진 상쇄(margin-collapsing)

인접 형제 요소간의 margin은 겹쳐서 보인다.

## 블록 레벨 요소와 인라인 레벨 요소

- 블록 레벨 요소 → div, ul, ol, li, p, hr, form
- 인라인 레벨 요소 → span, a, img, input, label, b, em, i, strong

## display

- display: block
    - 줄 바꿈이 일어나는 요소
    - 화면 크기 전체의 가로폭을 차지한다
    - 블록 레벨 요소 안에 인라인 레벨 요소가 들어갈 수 있다
- display: inline
    - 줄 바꿈이 일어나지 않는 행의 일부 요소
    - content 너비만큼 가로폭을 차지한다
    - width, height, margin-top, margin-bottom을 지정할 수 없다
    - 상하여백은 line-height로 지정한다
- display:inline-block
    - block과 inline 레벨 요소의 특징을 모두 갖는다
    - inline처럼 한 줄에 표시가 가능하다
    - block 처럼 width, height, margin 속성을 모두 지정할 수 있다