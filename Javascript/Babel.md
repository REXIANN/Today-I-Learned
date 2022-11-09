# Babel

출처: 유튜브-우테코
태그: 바벨

[[10분 테코톡] 나인의 Babel](https://www.youtube.com/watch?v=o-5K5Sc7L1k)

## 바벨 사용법

`const fn = () => "hello"` 를 트랜스파일링 하기 위해서 필요한 바벨 모듈

- `@babel/core` : 바벨의 핵심적인 기능
- `@bable/cli` : 터미널로 바벨을 사용
- `@babel/plugin-transform-arrow-functions` : 화살표함수를 transform 하는 플러그인

바벨을 사용할 경우 다음 명령어로 실행할 수 있다.

```bash
./node_modules/.bin/babel [변환할파일] --out-dir [변환될위치] --plugins=@babel/plugin-transform-arrow-functions
```

다음과 같이 변환된다.

```jsx
var fn = function fn() {
	return "hello";
};
```

하지만 다양한 플러그인을 설치하거나 매번 복잡한 명령어를 사용하기는 부담스럽다. 그래서 보통은 설정파일(`babel.config.json` 또는 `.babelrc.json` 파일. 두 개의 차이점에 대해서는 뒤에서 자세히 알아봄)을 사용하거나 preset 을 활용한다.

preset은 자주 사용되는 함수들을 모아둔 집합이다. preset 을 활용하면 매번 번거롭게 플러그인을 사용할 필요 없이 preset 에서 꺼내서 사용할 수 있다.

## Polyfill

polyfill 은 “충전솜” 이라는 뜻을 가지고 있다. 인형을 오래 쓰다 보면 속이 비게 되는데, 이를 채워주는 역할을 한다. 마찬가지로 바벨의 부족한 점을 채워주는 역할을 하는 것이 바로 폴리필(polyfill) 이다. 

> A polyfill is a piece of code(usually JavaScript on the Web) used to provide modern functionality on older browsers that do not natively support it.
> 

폴리필은 구현 브라우저에서 자체적으로 지원되지 않는 최신 기능들을 지원하고자 가져오는 코드 뭉치 라고 볼 수 있다. 

바벨은 preset 과 plugin 들을 가지고 트랜스파일링을 하게 된다. 하지만 트랜스파일링을 했다고 해서 모든 기능들을 다 사용할 수 있는 것이 아니다. 예를 들면 promise 같은 빌트인 객체들, `Array.prototype.includes` 같은 메서드들은 트랜스파일링을 하더라도 코드가 변하지 않고 남아있는 경우가 있다. 

만일 사용하고자 하는 브라우저에서 이러한 코드들을 지원하는 객체가 없다면 문제가 발생할 수 있다. 폴리필은 런타임에서 타겟 환경에 빌트인 메서드가 존재하지 않으면 이를 확인하고 부족한 코드들을 채워준다.

그래서 예전에는 `@babel/polyfill` 을 사용했다. 그러나 babel 7.4.0 부터는 deprecated 되었다. 대신 core-js 라는 것을 사용한다고 한다. 

또한 `@babel/polyfill` 이 사라지면서 `useBuiltIns` 옵션이 들어오게 되었다. `useBuiltIns` 와 `core-js` 버전명시 를 같이 사용해주게 되면 import 를 할 때 조금 더 효과를 볼 수 있다.

```json
// babel.config.json
{
	"presets": [
		["@babel/preset-env",
			{
				"useBuildIns": "entry",
				"corejs": "3.22"
			}
		]
	]
}
```

- `useBuiltIns: entry` : core-js/stable 과 regenerator-runtime/runtime 모듈을 전역 스코프에 직접 삽입한 경우 이를 타깃 환경에 필요한 폴리필만 전역 스코프에 추가되도록 변경
- `useBuiltIns: usage` : 실제 필요한 폴리필만 삽입. import 자체를 삽입해주어 따로 삽입할 필요가 없음

core-js 만 단독으로 사용하게 되면 별로 좋지 못하다. 왜냐하면 다른 메서드들(ex: Array.prototype.includes) 를 가져오면 전역스코프로 사용하게 되는데, 이는 전역스코프의 오염을 발생시킨다. 이름 충돌도 발생할 수 있고, 외부에서 사용하는 라이브러리가 전역스코프를 오염시키면 좋지 못하다. 그래서 이 기능은 보통 `@babel/plugin-transform-runtime` 이라는 플러그인과 같이 사용하여 전역스코프 오염을 막는다.

하지만 이것도 문제가 될 수 있다. 만약 프로젝트 내 패키지 중 전역스코프에 의존하는 패키지가 존재한다면 어떻게 할까? axios 는 전역 스코프에 있는 Promise 에 의존하고 있는데 이를 어떻게 해결할 수 있을까?

그래서 전역스코프에 의존성이 있는 패키지가 있다면 트랜스파일링의 대상에 포함시켜야 한다. 이때 필요한 것이 설정 파일이다.

## 바벨 설정 파일

### babel.config.json

- 여러 패키지 디렉토리를 가진 프로젝트에서 하나의 바벨 설정을 적용하고 싶을 때
- node_modules 도 적용하고 싶을 때

axios 라이브러리를 사용해야 한다면 config 파일에 넣어야 한다.

### babelrc.json

- 프로젝트 내에서 서드 파티 라이브러리가 바벨에 의해 트랜스폼되기를 바라지 않는 경우
- 특정 부분만 적용하고 싶은 경우

## Webpack 에서의 Babel

어느정도 규모가 있는 자바스크립트 프로젝트에서는 웹팩 같은 모듈 번들러를 사용한다. 웹팩은 각 파일의 의존성을 파악해서 몇개의 압축파일로 만들어준다. 그리고 웹팩과 바벨을 연결시켜주는 것이 babel-loader 이다. 바벨은 7버전부터는 타입스크립트도 트랜스파일링 할 수 있도록 지원해준다.

### babel-loader vs ts-loader

문자열에 숫자를 할당하는 경우 `const str: string = 9;` 

- 바벨: 에러ㄴㄴ
- TSLOADER: 에러ㅇㅇ

바벨은 타입스크립트의 모든 코드를 트랜스파일할 수 는 없(었)다. 하지만 2021년 7월 26일 출시된 Babel 7.15 버전부터는 모든 타입스크립트 코드 베이스를 컴파일 할 수 있다고 한다.

아직은 타입스크립트의 데코레이터(decorator) 는 변환하지 못한다. 왜냐하면 타입스크립트의 데코레이터는 아직까지도 많은 변화가 있는 기능이기 때문이다. 바벨은 ECMAScript spec 을 기준으로 하다보니 데코레이터를 타입스크립트가 원하는 대로 완전히 컴파일하지는 못한다. 하지만 관련 플러그인을 사용하면 된다.

빌드 속도는 babel-loader 가 ts-loader 보다 빠르다고 한다.