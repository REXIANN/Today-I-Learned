# webpack & babel & polyfill

웹팩은 모듈 번들러. 우리가 모듈 형태로 파일을 만들면 웹팩이 모아준다

```bash
npm install webpack webpack-cli
# start
npx webpack
```

실행하면 dist 폴더가 생기고 그 안에 번들링된 파일들이 들어간다. 

웹팩은 ESM, CommonJS 둘다 잘 처리해준다. 그래서 CJS 방식으로 작성된 라이브러리도 사용할 수 있다. ESM에서는 js파일만 가져올 수 있는데 웹팩을 사용하면 다양한 형식을 가져올 수 있다. 예를 들면 json파일 임포트가 있다. 기타 이미지 파일 등의 값을 원하는 현태로 자바스크립트로 다룰 수 있도록 해준다. 

또한 웹팩은 lodash같은 라이브러리를 설치하고 사용하는 코드를 작성하면 웹팩이 이 lodash의 내용을 번들파일에 포함시켜준다. 그래서 lodash가 ESM 이든 CJS 이든 오래된 브라우저에서 잘 작동한다. 

## 바벨

바벨은 입력과 출력이 모두 자바스크립트인 컴파일러이다.

초기에는 2015년에 나온 최신 자바스크립트문법을 기존 문법으로 변환해주는 용도로 사용되었다. 

지금은 코드압축이나 리액트의 JSX처럼 자바스크립트 문법이 아닌 코드를 자바스크립트 표준 문법으로 변환해주는 용도로 사용되기도 한다.

```bash
npm install @babel/core @babel/cli @babel/plugin-proposal-optional-chaining

# 실행
npx babel --plugins @babel/plugin-proposal-optional-chaining src/index.js

# index.js
const person = { name: 'mike' }
const age = person?.age
```

## 폴리필

바벨과 폴리필은 오래된 브라우저를 지원하는 용도로 사용될 수 있다. 

차이점은 바벨은 컴파일 타임에 실행되고 폴리필은 런타임에 실행된다는 점이다. 폴리필은 런타임에 사용자 환경을 보고 필요할 때에만 기능을 주입한다.

core-js 는 폴리필에 자주 사용되는 라이브러리이다. 

```jsx
import 'core-js/features/array/flat';
const arr = [1, [2, 3], [4, [5]]];
arr.flat(2);
```

폴리필은 실행시 flat 메서드가 없는 브라우저라면 해당 기능을 주입한다.