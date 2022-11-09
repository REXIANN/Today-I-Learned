# JS의 어레이 헬퍼 메서드 -arrayHelperMethods

출처: 엘리드림코딩
태그: 어레이헬퍼메서드

- map

```jsx
['1', '2', '3'].map(Number) // [1, 2, 3]

const numbers = [1, 2, 3]
const newNumbers = numbers.map(function(number) {
	return number + 1
}
console.log(newNumbers) // [2, 3, 4]
```

- forEach

```jsx
let sum = 0
[1, 2, 3].forEach(function(number) {
	sum += number
})

console.log(sum) // 6 
```

- filter

전체를 순회하며 조건을 만족하는 요소들을 찾음

```jsx
const oddNumbers = [1, 2, 3].filter(function(number) {
	return number % 2 === 1
})

console.log(oddNumbers) // [1, 3]
```

- find

조건을 만족하는 요소를 찾으면 return

```jsx
const arr3 = [1, 2, 3, 4, 5]
const number = arr3.find(num => num === 3) // 배열이 아닌 값을 리턴
```

- some

요소들 중 하나라도 조건을 만족하면 true, 아니면 false

```jsx
const arr = [10, 20, 30]
const isBiggerThan20 = arr.some(num => num > 20) 
console.log(isBiggerThan20) // True
```

- every

요소들 모두가 조건을 만족하면 true, 아니면 false

```jsx
const arr = [10, 20, 30]
const allBiggerThan5 = arr.every(num => num > 5) // true
const allBiggerThan11 = arr.every(num => num > 11) // false
```