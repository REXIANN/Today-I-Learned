# package-lock.json 은 뭘까?(feat. yarn.lock)

출처: 유튜브-기타
태그: npm

[package-lock.json 은 뭘까요?](https://www.youtube.com/watch?v=P2CtFD6xa54)

개발을 사용하다 보면 사용하는 라이브러리들을 관리하거나 세팅을 하기 위해서 npm 또는 yarn 을 사용하게 된다. 그러면 자연스럽게 package.json 파일을 보게 되는데, package.json 파일 말고도 package-lock.json 또는 yarn.lock 이라는 파일도 같이 생성, 수정 되는 것을 볼 수 있다. 이 파일들은 대체 왜 생기는 걸까?

## 발단

현대적인 언어는 보통 권장되는 패키지 관리 매니저는 하나씩 가지고 있다(Node.js → npm, yarn, pnpm, Rust → Cargo, Go → dep, Python → pip 등). 자바스크립트가 조금 예외적인 경우인데, 이미 npm 이라는 매니저가 있음에도 불구하고 yarn, pnpm 등의 다른 패키지 매니저들이 추가로 존재한다. 

이런  패키지 매니저들은 패키지 파일만이 아니라 **패키지락** 과 같은 락(lock)파일도 가지고 있다. 락 파일은 해당 프로젝트에 설치한 패키지, 그리고 설치한 패키지와 관련된 모든 패키지들의 버전 정보를 포함한다.

## package

다음과 같이 하나의 프로젝트에 두 개의 패키지가 설치되어 있다고 생각해보자.

```bash
npm install --save @package/A @package/B

@package/A => version 1.0
@package/B => version 2.1
```

해당 프로젝트를 다른 개발자가 다운로드 받아서  설치하면 두 개의 패키지가 설치된다. 그러면 보통 실행이 되지만 가끔 안될 경우도 있다. 그래서 원작자에게 문의하면 원작자는 “어? 저는 실행이 잘 되는데요??” 라고 할 때가 있다. 그럼 무엇이 문제일까?

```bash
npm install --save @package/A @package/B

@package/A => version 1.0
  >  @package/C (version *)
@package/B => version 2.1
```

아까 버전정보에서 부족했던 점은 패키지 A 가 가지고 있는 패키지 C 의 정보를 포함하지(명시하지) 않았기 때문이다. 또한 지금은 C 패키지의 경우 아무 버전이나 깔으라고 명시를 했다. 이러면 달라진 하위 패키지의 정보에 따라 실행이 되지 않을 수도 있다.

락파일은 패키지들과 연관된 다른 모든 패키지들의 정보(버전을 포함한)를 관리함으로써 다른 사용자가 설치를 할 때 숨겨진 의존성 문제를 해결한다. 

이러한 패키지를 데이터 구조에 빗대어 보면 그래프 와 비슷하다. 하나의 패키지를 노드로 보고, 하위 패키지들은 연결되는 엣지라고 해보자. 트리 구조랑 비슷하지 않을까?

```bash
- @package/A
	- @package/C
	- @package/D
- @package/B
	- @package/E
- @package/F
	- @package/G
	- @package/H
	- @package/K
```

하지만 트리 구조로 보기에는 약간 무리가 있다. 왜냐하면 실제로는 이런 경우가 허다하기 때문이다.

```bash
- @package/A
	- @package/C
	- @package/D  # shared!!
		- @package/E # shared!!
- @package/B
	- @package/D # shared!!
		- @package/E # shared!!
- @package/F
	- @package/G
	- @package/H
	- @package/K
```

이렇게 패키지들이 참조하는 패키지들이 겹칠 수 있기 때문이다. 그럼 이제 방향이 있는 네트워크 그래프처럼 볼 수 있다.

패키지 사이클이 생길 수도 있는데, 우선은 본 적이 없는 것 같다. 순환참조가 생길지 한번 다음에 시도를 해보는 것도 좋을 수도?