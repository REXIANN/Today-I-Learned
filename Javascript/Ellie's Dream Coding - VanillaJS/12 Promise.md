# 12. Promise

callback → promise → async await에 이르기까지

## Promise란 무엇인가?

한국어로는 약속 이라는 의미. JS에서 제공하는 비동기를 간단하게 처리할 수 있는 **오브젝트**이다. 비동기적인 기능이 정상적으로 처리되었다면 성공메시지와 함께 처리된 결과를 반환하고, 기능을 수행하다가 문제가 발생했다면 에러를 전달해준다. 어떻게 콜백을 사용하지 않고 Promise를 사용하여 깔끔하게 코드를 짤 수 있을까?

Promise는 자바스크립트에 내장된 오브젝트이다. 비동기적인 동작을 수행할 때 콜백함수 대신에 사용할 수 있다.

Promise는 state와 producer&consumer의 차이 를 잘 알아야 한다. State는 상태, 즉 해당 프로미스가 작업을 수행 중인지 혹은 작업을 끝내고 결과를 반환하는지에 대한 상태정보를 알아야 한다는 것이고, 두번째는 이 프로미스가 정보를 제공하는 provider의 역할인지, 아니면 정보를 받아 역할을 수행하는  consumer 역할인지 이해해야 한다. 

### State

작업이 진행중일 떄에는 펜딩(pending) 상태라고 하며, 작업이 완료된 상태는 성공의 경우 fulfilled, 작업이 실패한 경우 rejected 가 된다.

## Producer vs Consumer

## 1. Producer

프로미스는 클래스 이기 때문에 new라는 키워드를 사용하여 오브젝트를 만들 수 있다. 프로미스의 생성자를 보면 executor라는 콜백함수를 매개변수로 받고 executor는 또다시 다른 두개의 콜백함수를 매개변수로 받는다. 바로 resolve와 reject인데 resolve는 기능을 성공적으로 수행했을 경우 실행되는 콜백함수 이고 reject는 기능을 수행하다가 중간에 문제가 생기면 호출하게 될 콜백함수이다. 

```jsx
const promise = new Promise((resolve, reject) => { 
	// heavy stuffs here
	resolve(value);
	reject(new Error('no network');
});
```

네트워크 통신, 파일 전송 및 다운로드 등 시간이 꽤 걸리는 작업은 전부 비동기로 진행. Promise를 만드는 순간(생성하는 순간) executor 함수가 바로 실행된다. 즉 사용자가 요청을 했을 때에만 연결이 필요하다면 다른 방법을 사용해야 한다. 새로운 프로미스가 만들어 질 때에는 executor 함수가 자동적으로 실행이 된다. 

## 2. Consumers: then, catch, finally

위의 promise가 값을 가져왔다면 이제 그 값을 사용하기 위해 기다리던 소비자(consumer)가 나설 차례이다. consumer는 then, catch, finally 등의 메서드를 사용하여 작업을 한다.

```jsx
promise.then((value) => {
	console.log(value)
})
```

resolve라는 콜백함수를 통해 전달한 값은 then의 전달인자로 넘어오게 된다. 

에러가 났을 경우에는 reject를 사용하게 되는데 reject의 경우 안에 전달인자로 Error 오브젝트를 넣어준다. 이 Error오브젝트 또한 자바스크립트에서 기본적으로 지원하는 클래스이다. 이 에러 클래스에 대해 더 자세한 건 Error Handling 파트에서 보기로 하고 지금은 그냥 넘어가자. 

```jsx
promise.catch(error => {
console.log(error)
})
```

then은 똑같은 promise를 반환하기 때문에  .then 뒤에 .catch를 쓸 수 있다. promise 역시 메서드 체이닝이 가능하다. 

`finally` 의 경우 성공 또는 실패에 상관없이 실행된다. 

```jsx
.finally(() => {
conosle.log("finally!");
})
```

## 3. Promise Chaining

```jsx
const fetchNumber = new Promise((resolve, reject) => {
	setTimeout(() => resolve(1), 1000);
});

fetchNumber 
.then( num => num * 2 )
.then( num => num * 3 )
.then( num -> {
	return new Promise((resolve, reject) => {
		setTimeout(() => resolve(num - 1), 1000);
	});
})
.then(num => console.log(num));
```

여러개의 .then을 사용하여 일련의 작업들을 사슬화 할 수 있다. 

4. Error Handling

```jsx
const getHen = () => new Promise((resolve, reject) => {
  setTimeout(() => resolve('chicken'), 1000);
})

const getEgg = hen => new Promise((resolve, reject) => {
  setTimeout(() => resolve(`${hen} => egg`), 1000);
})

const cook = egg => new Promise((resolve, reject) => {
  setTimeout(() => resolve(`${egg} => cook!`), 1000)
})

getHen() //
  .then(getEgg)
	.catch(err => return 'error handling')
  .then(cook)
  .then(console.log);
```

전달인자가 한 개 이고 그 전달인자를 다음 메서드가 사용할 것이 자명하다면 전달인자를 생략하고 메서드만 전달인자로 넣어줄 수 있음!