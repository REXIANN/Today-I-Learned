# 8. Array와 APIs

# 8. Array와 APIs

자바스크립트에서 array를 제대로 활용하는 것은 정말 정말 중요하다!

## Array

자바스크립트에서 쓰이는 자료구조 중 하나

1. 배열 선언

```
const arr1 = new Array()const arr2 = []
```

1. Index position

```
const fruits = ['apple', 'banana']console.log(fruits) // ['apple', 'banana']console.log(fruits.length) // 2console.log(fruits[0]) // 'apple'console.log(fruits[1]) // 'banana'console.log(fruits[fruits.length - 1]) // 'banana'console.log(fruits[2]) // undefined
```

1. Looping over an array

```
const fruits = ['apple', 'banana', 'cherry']fruits.forEach((fruit) => console.log(fuit))fruits.forEach(function (fruit, index, array) {  console.log(fruit, index, array) // apple, 0, fruits배열, banan, 1, fruits배열})
```

1. Addition, deletion, copy

```
const fruits = ['apple', 'banana', 'cherry']// add an item from the endfruits.push('peach') // ["apple", "banana", "cherry", "peach"]fruits.pop() // ["apple", "banana", "cherry"]// add an item from the beginningfruits.unshift('peach') // ["peach", "apple", "banana", "cherry"]fruits.shift() //// ["apple", "banana", "cherry"]// shift와 unshift는 pop, push에 비해 느리다! // array가 기본적으로 스택 구조이기 때문에 shift는 인덱스 재정렬이 필요하기 때문!
```

splice

```
const fruits = ['apple', 'banana', 'cherry', 'peach', 'lemon']// 1번 인덱스부터 2개를 지우겠다는 뜻. 2를 안쓰면 다 지운다.fruits.splice(1, 2) // ['apple']fruits.splice(1, 1) // ['apple', 'cherry', 'peach', 'lemon']fruits.splice(1, 1, 'coke') // ['apple', 'coke', 'cherry', 'peach', 'lemon']fruits.splice(1, 0, 'coke') // ['apple', 'banana', 'coke', 'cherry', 'peach', 'lemon'] 삭제는 하지 않고 추가만 할 수도 있다!const foods = ['yogurt', 'pizza']const allFoods = fruits.concat(foods) // ['apple', 'banana', 'cherry', 'peach', 'lemon', 'yogurt', 'pizza']
```

1. Searching

```
const fruits = ['apple', 'banana', 'cherry', 'peach', 'lemon']fruits.indexOf('apple') // 0fruits.indexOf('cherry') // 2fruits.indexOf('coconut') // -1, 없는 원소를 찾으면 -1를 반환한다fruits.includes('peach') // truefruits.includes('coconut') // falseconst fruits2 = ['apple', 'apple', 'cherry', 'peach', 'lemon']fruits.indexOf('apple') // 0, 조건에 맞는 원소를 찾으면 그 인덱스를 반환fruits.lastIndexOf('apple') // 1, 조건에 맞는 마지막 원소를 찾아 그 인덱스를 반환
```