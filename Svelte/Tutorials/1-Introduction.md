# Svelte Introduction
## Svelte 란 무엇인가?
Svelte(이하 스벨트) 는 빠른 웹 애플리케이션을 구축하기 위한 도구입니다. 

스벨트는 React 나 Vue 와 같이 대화형 사용자 인터페이스를 쉽게 구축할 수 있도록 도와줍니다.

가상 DOM 을 사용하는 React 나 vue 와 다르게, 스벨트는 런타임에서 애플리케이션 코드를 해석하지 않습니다.
대신 프로젝트를 빌드할 때 앱 전체를 Javascript 로 변환합니다. 

다시 말해 브라우저에 올라가는 코드는 순수한 자바스크립트이며, 앱이 처음 로드될 때 프레임워크의 추상화에 대한 작업이 필요하지 않습니다.

이는 스벨트가 스스로를 "컴파일러" 라고 소개하는 이유이기도 합니다.

브라우저에서 가상 DOM 을 만들어서 상태가 변경될때마다 diffing 을 실행하는 대신 컴파일된 코드를 기반으로 실제 DOM 에 빠르게 변경사항을 적용합니다.
이것은 스벨트를 CDN 형식으로 불러서 런타임에 사용하는 것이 불가능한 이유입니다.

스벨트로 전체 앱을 빌드하거나, 기존의 코드베이스에 점진적으로 추가하는 방법도 가능합니다.
기존의 프레임워크에 대한 의존성 없이, 어디서나 작동하는 독립된 형태의 실행형 패키지로 컴포넌트를 제공할 수 있습니다. 

기존 코드베이스에 스벨트를 점진적으로 추가하는 방법은 나중에 더 자세히 알아보도록 하겠습니다.

## 스벨트 컴포넌트 이해하기
스벨트에서 애플리케이션은 하나 이상의 컴포넌트로 구성됩니다.

여기서 컴포넌트란 하나의 재사용 가능한 코드 블럭을 의미하며, 이 코드블럭이란 하나의 `.svelte` 파일 안에  캡슐화 되어 있는 HTML, CSS, 그리고 JS 파일을 의미합니다.

## 스벨트 기본 문법 
시작하기에 앞서 svelte 를 설치해야 합니다.
공식 문서에서 추천하는 방법은 빌드 도구 Vite 가 포함된 Sveltekit 을 사용하는 것 입니다.

다음 명령어를 통해 sveltekit 을 설치할 수 있습니다.
```shell
npm create svelte@latest myapp
cd myapp
npm install
```

**주의**: sveltekit 을 사용하려면 Node 버전이 16.12 이상이거나, 18 이상이어야 합니다.(17버전은 지원하지 않습니다.)
사용하는 Node 의 버전이 맞지 않으면 `npm install` 에서 에러가 날 수 있습니다.
저는 8버전과 16버전 두 개를 사용하고 있어서 다음과 같은 에러가 나옵니다. 

```shell
  ~/Documents/compare-svelte-to-react/myapp2 ❯ npm install                                                                                             system  17:45:01
npm ERR! code EBADENGINE
npm ERR! engine Unsupported engine
npm ERR! engine Not compatible with your version of node/npm: @sveltejs/kit@1.1.1
npm ERR! notsup Not compatible with your version of node/npm: @sveltejs/kit@1.1.1
npm ERR! notsup Required: {"node":"^16.14 || >=18"}
npm ERR! notsup Actual:   {"npm":"8.1.2","node":"v16.13.1"}
```

노드 버전을 18로 올리고 계속 진행하겠습니다.

### 데이터 추가하기

모든 스벨트 파일은 `.svelte` 확장자를 가지고 있어야 합니다. 
