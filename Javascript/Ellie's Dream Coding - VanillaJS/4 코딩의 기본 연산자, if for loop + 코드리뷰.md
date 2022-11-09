# 4. 코딩의 기본 연산자, if for loop + 코드리뷰 팁

# 4. 코딩의 기본 연산자, if for loop + 코드리뷰 팁

## 지난시간 복습

원시타입의 경우 값 자체가 메모리에 저장된다.

반면 오브젝트 타입의 경우 메모리에 한번에 올릴 수가 없고 레퍼런스가 있고 레퍼런스가 실제 값이 있는 주소의 값을 가지고 있다(가리키고 있다).

원시타입의 경우 `object.freeze()` 를 통해서 포인터를 잠글 수 있다(Immutable하게 만들 수 있다).

자바스크립트에서 기본적으로 모든 오브젝트 타입은 mutable이다.

### operator

1. String concatenation

```
console.log('my' + 'cat')console.log('1' + 2)console.log(`string literals: 1 + 2 = ${1 + 2}`)
```

1. Numeric Operators

```
console.log(1 + 1) // addconsole.log(1 - 1) //substractconsole.log(1 * 1) // multiplyconsole.log(1 / 1) // divideconsole.log(5 % 2)  // remainderconsole.log(2 ** 3) //exponentiation
```

1. Increment and decrement operators

```
let counter = 2const preIncrement = ++counter // 3conosle.log(`${preIncrement} ${counter}`) // 3, 3let counter = 2const postIncrement = counter++console.log(`${postIncrement} ${counter}`) // 3, 2
```

1. Assignment operators

```
let x= 3let y = 6x += y // 9x -= y // -3
```

1. Comparison operators

```
console.log(10 < 6) // less than, ltconsole.log(10 <= 6) // less than or equal, lteconsole.log(10 > 6) // greater than, gtconsole.log(10 >= 6) // greater than or equal, gte
```

1. Logical operators

```
const value1 = true;const value2 = 4 < 2; // false// ||(or), &&(and), !(not)console.log(value1 || value2) // trueconsole.log(value1 && value2) // falseconsole.log(!value1) // true// && 연산자의 경우 null-check가 가능하다! null은 false로 형변환이 되기 때문에 실행조건을 맞출 떄 쓰기도 한다.// && 연산자의 경우 모든 조건을 따지게 되므로 순서를 고려하여 작성하자!
```

1. Equality

```
const stringFive = '5'const numberFive = 5// == loose equality, with type conversion// 묵시적 형변환을 했을 때 같다면 같은 것으로 취급console.log(stringFive == numberFive) // true// === strict equality, no type conversion// 묵시적 형변환을 허용하지 않음. 정확한 형변환을 위해 이걸 쓰는것을 권장console.log(stringFive === numberFive) // false//equality - puzzlerconsole.log(0 == false) // trueconsole.log(0 === false) // falseconsole.log('' == false) // trueconsole.log('' === false) // falseconsole.log(null == undefined) // trueconsole.log(null === undefined) // false// 묵시적 형변환을 사용하면 3단논법이 먹히지 않는 이상한 자바스크립트의 유연성(?)을 볼 수 있다const a = '0'const b = 0const c = [] conosole.log(a == b) // trueconsole.log(b == c) // trueconsole.log(a == c) // false
```

1. Conditional operators: if

```
const name = 'kim'if (name === 'kim') {  console.log(`Welcome, ${name}`)} else if (name === 'Sam') {  console.log(`Welcome, ${name}`)} else {  console.log(`Who is this?`)}
```

1. Ternary operator: ?

```
// condition ? true-value : false-value;const condition = 'Hello'console.log(condition === 'Hello'? 'World': 'Everybody')
```

1. Switch statement

```
const browser = 'IE'switch( browser) {  case 'IE':    console.log('Unsupported browser')    break;  case 'Chrome':    console.log('Welcome!')    break;  case 'Firefox':    console.log('Welcome!')    break;  default:    console.log('Hi there')    break;}// 추후 타입스크립트에서 타입검사를 위해 스위치를 쓰는것이 좋다
```

1. Loops

```
let i = 3while (i > 0) {  console.log(`while: ${i}`)  i--;}let i = 3do {  console.log(`do while ${i}`)  i--;} while (i > 0);
```

1. For loop

```
// for (시작조건, 종료조건, 반복조건)for (let i = 0; i < 5; i++) {  console.log(`for: ${i}`)}for (let i = 5; i > 0; i -= 2) {  console.log(`inline variable for: ${i}`)}// nested loopsfor (let i = 0; i < 10; i++) {  for (let j = 0; j < 10; j++) {    console.log(`${i}, ${j}`)  }}
```