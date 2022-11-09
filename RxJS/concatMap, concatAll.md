# concatMap, concatAll

![Screen_Shot_2021-04-25_at_9.03.33_PM.png](./images/concatMap,%20concatAll/Screen_Shot_2021-04-25_at_9.03.33_PM.png)

### concatMap

다음 택배를 받기 전 개봉, 검사, 사용을 하고 나서 그 다음 택배를 받고 다음 택배에서도 동일한 로직을 사용한다고 가정해보자.

한마디로 택배가 올 때마다 할 로직들(개봉, 검사, 사용)을 매핑할것이고 이 일련의 작업들을 모든 택배에 이어붙이겠다!

```jsx

const Rx = require('rxjs')
const { concatMap } = require('rxjs/operators')

const stream = Rx.from([1, 2, 3, 4])

async function userTask(data) {
  await openBox(data)
  await checkBox(data)
  await useProduct(data)
}

stream.pipe(
  concatMap(data => Rx.from(userTask(data))) // 작업에 Task를 매핑한다
).subscribe()
```

map은 데이터의 흐름이 들어올때 각 데이터를 다른 옵저버블에 매핑해주는 역할을 한다!

### concatAll

all은 데이터의 흐름 자체가 또 다른 옵저버블의 흐름일때 사용한다. 옵저버블에 대한 옵저버블이라 조금 어렵게 느껴질 수도 있으니 예제를 같이 보자.

```jsx
const stream1 = Rx.interval(1000).pipe(take(3))
const stream2 = Rx.interval(1000).pipe(take(3))

const stream3 = Rx.of(stream1, stream2)

stream3.subscribe(console.log) // 옵저버블들이 나온다
// Observable {
//   _isScalar: false,
//     source: Observable { _isScalar: false, _subscribe: [Function (anonymous)] },
//   operator: TakeOperator { total: 3 }
// }
// Observable {
//   _isScalar: false,
//     source: Observable { _isScalar: false, _subscribe: [Function (anonymous)] },
//   operator: TakeOperator { total: 3 }
// }
```

각 스트림이 끝날 때 마다 다음 작업을 시키고 싶을때 사용하면 된다

```jsx
const stream1 = Rx.interval(1000).pipe(take(3), tap(console.log))
const stream2 = Rx.interval(1000).pipe(take(3), tap(console.log))

const stream3 = Rx.of(stream1, stream2)

stream3.pipe(
  concatAll()
).subscribe()
```

여러개의 옵저버블에 대한 옵저버블들 안에서 그 안의 스트림들이 종료되고 순차적으로 이어붙이고 싶을 때 concatAll을 사용할 수 있따.