# .reduce를 활용한 다양한 연산

출처: 공부
태그: 어레이헬퍼메서드

어레이 헬퍼 메서드 중 하나인 `.reduce` 는 대개 배열의 덧셈을 위해 사용한다. 그 이유는 `.reduce` 메서드는 결과값을 다음 번 실행하는 콜백함수의 인자로 전달하기 때문이다. 우선 예제를 같이 보자.

### 배열의 덧셈

```jsx
let arr = [1, 2, 3, 4, 5]
const result = arr.reduce((acc, cur) => acc + cur)
console.log(result) // 15
```

두 개의 전달인자 `acc`, `cur` 은 각각 누적값, 현재값을 가리킨다. 현재 값인 `acc` 는 콜백함수에 이어 두번째 전달인자로 설정해주어야 하며 설정하지 않을 경우 기본값은 0 으로 지정된다. 이를 이용해서 초기값을 0으로 두고, 현재값과 누적값(현 상태에서는 초기값이 된다)을 더한 결과를 다음 동작의 누적값으로 전달해주어 최종적으로 배열의 합계를 낼 수 있다.

인덱스와 초기값을 추가하여 같이 보자.

```jsx
let arr = [1, 2, 3, 4, 5]
const result = arr.reduce((acc, cur, index) => { // 매개변수로 인덱스를 넣을 수 있다
	console.log(index, acc, cur) // index, acc, cur 값을 출력
	return acc + cur
}, 0) // acc의 초기값은 0 으로 지정해준다.
console.log(result)

0 0 1
1 1 2
2 3 3
3 6 4
4 10 5
15
```

콜백함수로 전달되는 누적값을 어떻게 세팅하느냐에 따라 다양하게 활용이 가능하다.

문자열을 합친다던지...

```jsx
let helloWorld = ["Hel", "lo", "Wo", "r", "ld!"]
let sayHelloWorld = helloWorld.reduce((acc, cur) => acc + cur, "")
console.log(sayHelloWorld) // HelloWorld!
```

`.map` 처럼 사용하는 것도 가능하다.

```jsx
let numbers = [2, 6, 11, 8, 9, 16, 21]
let isOddNumbers = numbers.reduce((acc, cur) => { 
  acc.push(cur % 2 ? "홀수": "짝수")
  return acc
}, [])
console.log(isOddNumbers)
// ['짝수', '짝수', '홀수', '짝수', '홀수', '짝수', '홀수']
```