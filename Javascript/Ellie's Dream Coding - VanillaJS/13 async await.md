# 13. async await

비동기의 마지막, async와 await에 대하여.

async&await은 promise를 좀 더 **간단하고 간결하고 동기적으로 실행되는 것 처럼 보이게** 만들어준다. Promise의 .then의 체이닝 대신 async&await 을 사용하면 동기식 처럼 간단하게 코드를 작성 가능하다.

async&await은 완전히 새로운 개념이 아니라 기존의 Promise개념 위에 조금 더 간편한 API를 제공하는 것이다. 이렇게 기존에 존재하는 것을 감싸서 새로운 기능을 더 제공하는 것을 syntactic sugar(문법적 설탕)이라고 한다.

또다른 syntactic sugar로는 클래스(Class)가 있다. 클래스는 프로토타입을 베이스로 하여 그 위에 덧붙여진, 그럴싸하게 보이는 녀석이다!

async&await은 promise를 깔끔하게 사용할수 있는 방법이다. 그렇다고 해서 promise가 나쁘거나 async await 으로 무조건 변환해서 사용해야 하는 것은 아니다. promise와 async&await을 상황에 맞게 적절하게 사용하는 것이 중요하다.

## 1. async

사용자의 정보를 가져오는데 10초가 걸리는 함수가 있다고 가정해보자

```jsx
function fetchUser() {
  // do network request for 10 sec...
	return 'user';
}

const user = fetchUser(); // 이 경우 여기서 10초가 소요된다.
console.log(user); 
```

유저 정보를 가져오는 동안(즉, fetchUser가 콜스택에 있는 동안) 브라우저는 아무 일도 하지 못하고 기다려야 한다. 지난 시간에는 함수의 반환값을 Promise 오브젝트로 만들어 주었었다. 이 promise안에는 resolve, reject라는 콜백함수를 받는 executor라는 콜백함수가 위치하여 코드 안에 있는 작업들이 비동기적으로 사용되게 한다. 그리고 resolve와 reject를 사용하지 않고 실행하면 promise의 상태가 pending으로 남아있는 것을 볼 수 있다. 

```jsx
function fetchUser() {
  return new Promise((resolve, reject) => {
		// do network request for 10 sec...
		resolve('user');
	});
}

const user = fetchUser();
user.then(console.log);
```

promise를 사용하지 않고도 함수를 비동기적으로 만드는 방법이 있다. 바로 함수 키워드 앞에 async 키워드를 붙여주는 것! 키워드를 작성하면 자동적으로 함수 안의 코드블럭들이 promise로 변환이 된다. 다시 말해 fetchUser는 이제 Promise객체를 반환한다.

```jsx
async function fetchUser() {
	return 'user';
}

const user = fetchUser();
console.log(user);

// 출력화면
Promise {<fulfilled>: "user"}
__proto__: Promise
[[PromiseState]]: "fulfilled"
[[PromiseResult]]: "user"
```

## 2. await

await이라는 키워드는 async 가 붙은 함수 안에서만 사용할 수 있다. await 이라는 키워드를 쓰면 해당 키워드가 있는 함수의 부분이 완료될 때 까지 기다린다.

예제를 통해 알아보자. 다음과 같이 사과와 바나나를 얻어오는 비동기 함수 두개가 있다. 과일 하나를 받아오려면 3초를 기다려야 한다.

```jsx
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(3000);
  return 'Apple';
}

async function getBanana() {
  await delay(3000);
  return 'Banana';
}
```

이렇게 짜면 사수한테 맞는다고 한다. 조심하자. 

```jsx
function pickFruits() {
  return getApple().then(apple => {
    return getBanana().then(banana => `${apple} and ${banana}`)
  });
}
pickFruits().then(console.log);
```

async 기능을 활용하면 간단하게 만들 수 있다. ~~뭐야 너무 깔끔해 멋있어....~~

```jsx
async function pickFruits() {
	const apple = await getApple();
	const banana = await getBanana();
	return `${apple} and ${banana}`
}
```

여기서 문제는 사과를 받아오는데 3초, 그리고 다시 바나나를 받아오는데 3초가 걸린다는 점이다. 두 개의 과일을 가져오는 작업은 서로 연관이 없기 때문에 서로의 결과를 기다릴 필요도 없다. 그렇다면 어떻게 하면 좋을까?

```jsx
async function pickFruits() {
	const applePromise = getApple();
	const bananaPromise = getBanana();
	const apple = await applePromise;
	const banana = await bananaPromise;
	return `${apple} and ${banana}`
}
```

코드를 보면 applePromise와 bananaPromise는 만드는 순간(컴파일러가 읽는 순간) 코드가 실행되므로 바로바로 실행이 된다. 그리고 apple 과 banana는 위에 있는 두개의 함수의 실행 결과값을 기다리게 하는 것이다. 이렇게 동시다발적으로 실행이 가능한 경우는 서로의 함수가 연관이 없어서 병렬적으로 기능을 수행해도 좋을 때 이다.

## 3. useful Promise APIs

이번에는 promise에 있는 유용한 기능을 사용하여 보자. Promise에는 .all이라는 api를 사용하면 promise 배열들 전달해 모든 promise의 결과가 받아지길 기다렸다가 받아진 배열을 전달해준다. 마찬가지로 코드를 통해 빠르게 이해해보자.

```jsx
function pickAllFruits() {
	return Promise.all([getApple(), getBanana()]).then(fruits => fruits.join(' and ');
}
pickAllFruits().then(console.log); // apple and banana
```

혹은 어떤 과일이든 상관없이 먼저 딸 수 있는 한 개의 과일만 필요하다고 할때는 Promise에 있는 .race라는 api를 사용할 수 있다. .race를 사용하면 가장 먼저 전달되는 값 하나만 전달이 된다. 말 그대로 race, 경주를 시키는 것 같다!

```jsx
function pickOnlyOne() {
	return Promise.race([getApple(), getBanan()]);
}
```

이렇게 자바스크립트에서 사용가능한 비동기 처리 방법 세가지, callback 함수와 Promise, async&await 까지 알아보았다.