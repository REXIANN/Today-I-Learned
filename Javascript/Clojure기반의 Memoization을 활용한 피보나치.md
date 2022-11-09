# Clojure기반의 Memoization을 활용한 피보나치 수열

출처: 공부
태그: 클로저

기존의 피보나치 수열의 경우 `f(n) = f(n - 1) + f(n - 2)` 형태를 띄며 실행시 2^n의 시간복잡도를 가지는.. 실생활에서 사용하는것을 지양해야할 점화식이다. 그러나 메모이제이션 기법을 사용하면 약간의 공간복잡도를 희생하여 피보나치 수를 구하는데 걸리는 시간복잡도를 선형시간(`O(n)`) 으로 만들어줄 수 있다. 그렇다면 자바스크립트에서는 어떻게 메모이제이션을 구현할까? 한번 알아보자.

먼저 평범한 피보나치 함수에 대해 살펴보자

```jsx
let count = 0;
let fib = function(n) {
	count++;
	return n < 2? n: fib(n - 1) + fib(n - 2);
}

for (let i = 0; i < 10; i++) {
	console.log("fib" + i + " = " , fib(i));
}
console.log(count); // 276
```

결과

```bash
➜  js-practice node fibo.js
fib0 =  0
fib1 =  1
fib2 =  1
fib3 =  2
fib4 =  3
fib5 =  5
fib6 =  8
fib7 =  13
fib8 =  21
fib9 =  34
276
```

결과를 보면 0부터 9 까지의 피보나치 수를 얻기 위해 250번이 넘는 연산이 실행되었음을 볼 수 있다. 그렇다면 이제 메모이제이션을 활용하여 값을 구해보자.

```jsx
let count = 0;
let fibo_memo = function(n) {
  let memo = [0, 1];
  let fibo = function(n) {
    count++;
    let result = memo[n];
    if (typeof result !== "number") {
      result = fibo(n - 1) + fibo(n - 2);
      memo[n] = result;
    }
    return result;
  }
  return fibo;
}();

for (let i = 0; i < 10; i++) {
  console.log('fibo_memo' + i + ' = ', fibo_memo(i));
}
console.log(count)
```

결과

```bash
➜  js-practice node fibo_memo.js
fibo_memo0 =  0
fibo_memo1 =  1
fibo_memo2 =  1
fibo_memo3 =  2
fibo_memo4 =  3
fibo_memo5 =  5
fibo_memo6 =  8
fibo_memo7 =  13
fibo_memo8 =  21
fibo_memo9 =  34
26
```

26번만 실행된 것을 볼 수 있다. 함수의 구조를 보면 우선 `fibo_memo` 로 입력을 받는다. 그 후 `fibo_memo` 가 가진 `memo` 어레이를 참고하여 값을 가져온다. 만약 해당 숫자가 어레이의 길이보다 길다면 값은 숫자가 아닌 `undefined` 가 될 것이므로 그때 어레이의 값을 찾아 넣어준다. 이때 `fibo` 함수의 재귀동작으로 `memo` 의 값을 채워넣는다.  이 경우 `memo`  라는 어레이를 하나 만들어야 하고 그 어레이에 값을 동적으로 하나씩 할당해 주어야 한다. 그렇기 때문에 기존의 피보나치보다는 길이가 입력에 따라 늘어나는 어레이 하나만큼의 공간이 필요하게 된다. 그러나 대신 연산의 수를 비약적으로 줄여준다는 점에서 의미있는 희생(?)이라고 볼 수 있다. 특히 공간복잡도는 추가적인 메모리 투입 등을 통해 쉽게 극복할 수 있는 반면 시간 복잡도는 그 무엇으로도 살 수 없는 시간을 희생해야 한다. 

또한 위의 메모이제이션 코드를 보면 클로저를 통해 `memo` 어레이에 접근하고 있음을 볼 수 있다. 클로저란 함수와 함수가 선언된 어휘적 환경의 조합으로 함수가 생성될때의 환경을 기억하였다가 실행될때 그 지역의 규칙에 따라 변수에 접근하고 사용하는 것으로 여기서 `fibo` 함수는 `memo` 어레이에 클로저를 통해 접근하게 된다.

클로저라는 중요한 개념도 알고는 있다고 생각하는데 막상 적으려니 매끄럽게 정리가 잘 되지 않는다. 자바스크립트 공부를 다시 시작해야지...