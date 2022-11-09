# 06/09(화) Vue+API 2부

Created: 2020년 10월 4일 오후 11:58
Tags: lifecyclehook

## LifeCycleHook

updated → 데이터가 업데이트되면 무조건 호출이 되게 되어 있음. 만약 input에 v-model로 엮여 있다면 한글자 칠때마다 실행될 것

## VueRouterGuard

`router.beforeEach` . 네이게이션 가드라고도 한다.

라우터에서 라우터를 시작하기 전에 어떤 콜백함수를 실행하여 어떤 것을 판별하고 조절한다. 

```jsx
// routes.js
{ 
	path: '/articles/create',
	name: 'Create',
	component: CreateView,
	beforeEnter(from, to, next) {
		// from과 to는 컴포넌트, next는 함수다
		console.log(from, to)
		if (!Vue.$cookies.isKey('auth-token')) {
			next('/accounts/login')
		} else {
			next()
		}
	}
}
```

뷰 라우터 네비게이션 가드로 예상밖의 행동을 방어하자