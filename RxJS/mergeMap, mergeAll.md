# mergeMap, mergeAll

![Screen_Shot_2021-04-25_at_9.19.45_PM.png](./images/mergeMap,%20mergeAll/Screen_Shot_2021-04-25_at_9.19.45_PM.png)

택배는 1초에 한개씩 오는데 택배를 개봉하고 검사하고 사용하는데 15초가 걸린다면...?

이러한 문제를 해결하기 위해 머지맵을 사용할 수 있다

### mergeMap

- 택배가 올 때마다 일련의 작업을 수행하는 것은 concat과 동일하다
- 택배는 택배대로 받고, 택배를 받으면서 모든 택배에 대해서 해당 작업을 수행을 해라!

즉, 택배를 받으면서 각 택배들이 거쳐야 하는 일련의 작업들이 동시에 실행되게 하는 것!

```jsx
const Rx = require('rxjs')
const { mergeMap, mergeAll, take, map } = require('rxjs/operators')

const stream1 = Rx.interval(1000).pipe(take(3), map(data => `1st: ${data} `))
const stream2 = Rx.interval(1000).pipe(take(3), map(data => `1st: ${data} `))

const stream3 = Rx.of(stream1, stream2)

stream3.pipe(
  mergeMap(data => Rx.from(userrTask(data)))
).subscribe()
```

concatMap은 데이터가 올 때마다 해당 데이터에 대한 작업이 될 때까지 다음 데이터를 수신하는 것을 미루고 작업을 이어간다면, mergeMap은 데이터스트림의 흐름을 방해하지 않으면서 각 비동기 작업을 동시에 실행시킨다.

### mergeAll

mergeALl은 여러개의 옵저버블을 동시에 실행시킨다

```jsx
const stream4 = Rx.interval(1000).pipe(take(3), tap(console.log))
const stream5 = Rx.interval(1000).pipe(take(3), tap(console.log))

const stream6 = Rx.of(stream4, stream5)

stream6.pipe(
  mergeAll()
).subscribe()
```

![Screen_Shot_2021-04-25_at_9.31.15_PM.png](./images/mergeMap,%20mergeAll/Screen_Shot_2021-04-25_at_9.31.15_PM.png)

일련의 작업 뭉치가 두개가 있고 각 작업들을 두개씩 묶어서 실행을 시키고 싶다면?

```jsx
const stream11 = Rx.interval(1000).pipe(take(3), tap(console.log))
const stream22 = Rx.interval(1000).pipe(take(3), tap(console.log))
const stream33 = Rx.interval(1000).pipe(take(3), tap(console.log))
const stream44 = Rx.interval(1000).pipe(take(3), tap(console.log))
const stream55 = Rx.interval(1000).pipe(take(3), tap(console.log))

const stream66 = Rx.of(stream11, stream22, stream33, stream44, stream55)

stream66.pipe(
  mergeAll(2)
).subscribe()
```

두개씩 머지해서 실행해라 라고 주면 됌!