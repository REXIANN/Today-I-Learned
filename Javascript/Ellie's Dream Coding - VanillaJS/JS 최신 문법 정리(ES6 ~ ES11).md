# JS 최신 문법 정리(ES6 ~ ES11)

ES6이후로 추가된 많은 문법들은 IE에서는 적용되지 않기 때문에 Babel을 사용해야 한다.

## ES6의 기능들

### Shorthand property names

오브젝트를 생성할 때 키와 값의 이름이 같다면 생략할 수 있다

```jsx
const name = 'kim';
const age = '29';

// ES5
const user = {
	name: name,
	age: age
}

// ES6
const user = {
	name, 
	age
}
```

### Destructuring assignment

오브젝트의 키와 값에 접근하기 위해서는 기존에 .을 사용해야 했었다. 하지만 Destructuring을 사용하면 오브젝트의 키에 할당된 값이 오브젝트의 키와 동일한 이름의 변수에 차례대로 적용되는 것을 볼 수 있다. 만일 이름을 바꾸고 싶다면 {}안에서 새로운 이름을 지정하면 된다. 나도 처음에는 이해가 잘 안되서 힘들었다. 예제를 우선 보자

```jsx
const user = {
	name: 'Terry',
	age: 41
}

// ES5
const name = user.name
const age = user.age
console.log(name, age) // 'Terry', 41

// ES6
const { name, age } = user;
console.log(name, age) // 'Terry', 41

const { name:userName, age:userAge } = user;
console.log(userName, userAge) // 'Terry', 41
```

 

배열에서도 동일하게 사용할 수 있다. 다만 이때 Destructured 되는 변수들 또한 배열로 감싸주어야한다.

```jsx
const animals = ['dog', 'cat'];
const [firstAnimal, seondAnimal] = animals;
console.log(firstAnimal, secondAnimal); // 'dog', 'cat'
```

### Spread syntax

오브젝트를 담고 있는 배열을 복사하기 위해서는 어떻게 해야 할까? 기존의 방법으로는 배열에 .forEach나 .map을 사용했었다. 그러나 이제는 `...` 를 사용하여 배열을 복사해 올 수 있다. 

```jsx
const obj1 = { key: 'firstKey' }
const obj2 = { key: 'secondKey' }
const arr = [obj1, obj2]

// array copy
const arr2 = [...arr];
const arr3 = [...arr, { key: 'thirdKey' }] // 원소의 추가도 가능하다

// object copy
const obj3 = { ...obj1 }

// array concatenation
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]
const array3 = [...array1, ...array2] // [1, 2, 3, 4, 5, 6]

// object merge
const dog1 = { dog: 'Hello' }
const dog2 = { dog: 'World' }
// 오브젝트의 키가 같으면 제일 뒤에 있는 키의 값으로 갱신된다. 주의!
const dog = { ...dog1, ...dog2 } // { dog: 'World' }
```

또한 오브젝트를 가리키고 있는 변수는 실제 오브젝트를 담고 있는 것이 아니라 오브젝트가 위치한 주소값을 가지고 있다. 그래서 이 방법으로 복사된 배열들의 안에 있는 오브젝트는 사실 **전부 같은 오브젝트**를 가리키고 있다. 이 점을 항상 유의하자.

### Default parameters

간단하다. 기본인자 설정이 가능하다.

```jsx
function printMessage(message = 'default') {
	console.log(message);
}

printMessage('hi'); // 'hi'
printMessage(); // 'default'

```

### Ternary Operator

그냥 삼항 연산자이다.

```jsx
const isOkay = true
const howAreYou = isOkay? 'fine' : 'not good';
console.log(howAreYou) // 'fine'
```

### Template Literals

내가 배울때에는 백틱 ``` 이라고 배웠다. 문자열 안에 변수를 넣어서 하나의 문자열로 편하게 만들 수 있다. URI를 구성하거나 숨겨놓은 API Key를 가지고 와서 request를 보낼때 많이 사용한다.

```jsx
const API_KEY = '1234';
const URL = 'http://hello.world.io/'

const req = `${URL}${API_KEY}` // 'http://hello.world.io/1234'
 
```

## ES11의 기능들

### Optional chaining

오브젝트 안에 또 오브젝트가 있는 경우에 사용가능하다. Kotlin과 Swift에도 포함된 기능이라고 한다!

```jsx
const person1 = {
	name: 'Kim',
	job: {
		title: 'Developer',
		boss: {
			name: 'Lee'
		}
	}
}

const person2 = {
	name: 'Lee'
}

// Before
function printManager(person) {
	console.log(person.job.boss.name);
}
printManager(person1) // Lee
printManager(person2) // Error!

// ES11
function printManager(person) {
  console.log(person.job?.manager?.name)
}
printManager(person1) // Lee
printManager(person2) // undefined
```

### Nullish coalescing operator

이것을 이해하기 위해 우리는 falsy의 개념에 대해 잘 알아야한다. JS에서 공식적으로 제공되는 false 뿐만 아니라 Boolean 타입으로 암묵적 형변환이 되었을 때 false를 가리키는 값이 6개가 있다. 

바로 false, '', 0, null, undefined, NaN 이다. 

~~근데 NaN은 왜 넘버 타입...~~  

~~null은 심지어 오브젝트 타입...~~

예제를 같이 보자. name이 존재할 경우 userName은 name이 되고, 만일 name이 없다면 'Guest'로 userName을 지정해주려고 한다. 하지만 이 경우에는 사용자가 이름을 빈문자열 `''` 이나 숫자 0으로 채운다면 똑같이 Guest라는 이름을 사용해버린다. 

```jsx
const name = null;
const userName = name || 'Guest'
console.log(userName); // Guest

const name = undefined;
const userName = name || 'Guest'
console.log(userName); // Guest

const name = '';
const userName = name || 'Guest'
console.log(userName); // Guest

const name = 0;
const userName = name || 'Guest'
console.log(userName); // Guest
```

이를 방지하기 위해 Nullish Coalescing(`??`)을 사용해주면 NaN, '', 0에 대해서 암묵적 형변환이 일어나지 않고 그대로 출력을 해준다. 나머지 두개인 null과 undefined는 그대로 false가 유지된다. 

```jsx
const name = null;
const userName = name || 'Guest'
console.log(userName); // Guest

const name = undefined;
const userName = name || 'Guest'
console.log(userName); // Guest

const name = '';
const userName = name ?? 'Guest'
console.log(userName); // ''

const name = 0;
const userName = name ?? 'Guest'
console.log(userName); // 0
```

암묵적 형변환을 활용한 유명한 개그를 마지막으로 포스트를 마친다

![Untitled.png](./images/JS%20최신%20문법%20정리(ES6%20~%20ES11)/Untitled.png)