# The secret to mastering CSS layouts

[The secret to mastering CSS layouts](https://www.youtube.com/watch?v=vHuSz4fRM88)

## Relationship between parents and children width and height

부모가 width 제한을 가지면 자식도 부모의 제한을 따라 가진다.  부모의 width 를 특정 픽셀로 제한하는 경우 자식들도 그 안에서 따라 가지게 된다.

하지만 자식컴포넌트(주로 텍스트) 들이 차지하는 영역은 일정하기에, 더 긴 height 를 가지게 되고, 이는 우리가 부모 컴포넌트의 height 를 설정했음에도 자식 컴포넌트들이 일정한 영역을 가지기 위해 지정한 height를 벗어나는(overflow) 하는 동작을 보여준다.

- `width: auto` 와 `width: 100%` 의 차이

  블록 엘리먼트에서의 width 에 대해 다룬다. 기본적으로는 `width: auto;` 가 적용되어 있다고 보면 된다. 그래서 브라우저의 길이를 줄이면 알아서 줄어든다(예제에서는 백그라운드 컬러를 넣은 `<p>` 태그를 사용했다). 여기서 왼쪽에 마진을 넣게 되면 왼쪽에 넣은 마진값을 가진다. 여전히 뷰포트에 따라 왼쪽에 정해진 마진을 가지고 나머지 영역을 가진다.

  `width:auto` 의 의미는 사용가능한 영역을 전부 사용한다는 뜻이다. 그래서 `margin: 0 auto;` 를 주면 컨텐츠가 가운데로 오는 것이다.

  하지만 `width:100%` 를 사용하면 부모컴포넌트의 넓이 전체를 사용한다는 것을 의미한다. 부모컴포넌트는 다른 블록 엘리먼트나 document 엘리먼트, body 엘리먼트 등이 될 수 있다. 만약 `width:100%` 를 가진 컴포넌트가 마진을 가지고 있다면 부모엘리먼트의 길이 전체를 길이로 가지고(고정값이다) 거기에 더해 좌우 마진을 넣게 된다. 이는 곧 좌우 스크롤이 생기는 주요 원인이 된다.