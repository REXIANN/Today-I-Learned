# 75. 하이오더 컴포넌트 구현하기

커링과 같이 함수 자체를 인자로 받거나 반환하는 함수를 “고차 함수" 라고 한다. 이와 비슷하게 컴포넌트를 인자로 받거나 반환하는 함수를 고차 컴포넌트(HOC, Higher-Order Component) 라고 한다.

```jsx
// App.js
import React from 'react'
import ReactHoc from './Hoc/R075_ReactHoc'

function App() {

	return (
		<div>
			<h1>Start React 200!</h1>
			<ReactHoc name='React200' />
		</div>
	)
}
```

```jsx
// Hoc/R075_ReactHoc.js
import React from 'react'
import withHocComponent from './withHocComponent'

class R075_ReactHoc extends React.Component {

	render() {
		console.log('2. HocComponent render')
		return (
			<h2>props.name : ${this.props.name}</h2>
		)
	}
}

export default withHocComponent(R075_ReactHoc, 'R075_ReactHoc')
```

마지막 줄에서 `withHocComponent` 컴포넌트를 호출하면서 `R075_ReactHoc` 컴포넌트와 컴포넌트 명을 파라미터로 넘긴다. 이 때 `R075_ReactHoc` 컴포넌트는 익스포트(export) 되지 않기 때문에 `render` 함수가 실행 되지 않는다.

```jsx
// Hoc/withHocComponent.js
import React from 'react'

export default function withHocComponent(InComponent, InComponentName){
	return class OutComponent extends React.Component{
		componentDidMount() {
			console.log(`3. InComponentName: ${InComponentName}`)
		}

		render() {
			console.log('1. InComponent render')
			return (<InComponent {...this.props} />)
		}
	}
}
```

withHocComponent.js 는 R075_ReactHoc.js 에서 익스포트 되면서 전달한 파라미터를 받는다.

파라미터로 전달받은 `InComponent` 변수는 `R075_ReactHoc` 컴포넌트 그 자체이다. 따라서 `withHocComponent` 는 `R075_ReactHoc` 컴포넌트를 반환(return) 하면서 App.js 로부터 받은 props 를 전달한다.

`withHocComponent` 의 `render` 에 있는 `InComponent`(`R075_ReactHoc`)가 리턴되면 `R075_ReactHoc` 컴포넌트의 `render` 함수가 실행되고 `props.name` 의 값이 화면에 출력된다.

## 결론

하이 오더 컴포넌트(HOC)를 구현하면, 여러 컴포넌트에 동일하게 적용되어야 하는 공통 기능을 코드 중복 없이 사용할 수 있다. withHocComponent.js 코드에서 예를 들면 `render` 함수 안에 있는 `console.log` 를 모든 컴포넌트에서 출력해야 하는데, 만약 hoc 를 구현하지 않았다면 각각의 컴포넌트에서 동일한 컴포넌트를 작성해야 한다.