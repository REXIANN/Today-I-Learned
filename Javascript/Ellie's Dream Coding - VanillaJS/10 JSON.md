# 10. JSON

# JSON

## 사전정리

HTTP란 Hypertext Transfer Protocol의 약자로 Client와 Server간의 통신의 약속. 클라이언트는 서버에게 데이터를 요청(request)하고 서버는 클라이언트에게 그에 맞는 응답(response)을 보낸다. 여기서 Hypertext란 웹사이트에서 사용되는 하이퍼링크(Hyperlink)뿐만 아니라 전반적으로 사용되는 리소스들(문서, 이미지 등)을 포함해서 말한다.

이렇게 HTTP를 이용해서 서버에서 데이터를 받아오는 방법으로는 AJAX가 있다. Asynchronous JavaAScript and XML의 약자로 웹페이지에서 동적으로 서버에게 데이터를 요청하고 받아오는 기술이다. 대표적인 예로는 XMLHttpRequest라는 오브젝트가 있다. 이 오브젝트는 브라우저 API에서 제공하는 오브젝트 중의 하나로 이 오브젝트를 사용하면 간단하게 서버에게 데이터를 요청하고 받아올 수 있다. 최근 브라우저 API에 추가된 fetch() API를 사용하면 역시 간편하게 데이터를 주고받을 수 있다. 하지만 무조건 신상이라고 해서 바로 사용할 수는 없다는 사실! 이 API는 인터넷 익스플로러에서 지원이 되지 않으니 유의해서 사용하자.

## XML

XML은 HTML과 같은 마크업 언어 중의 하나이다. 태그를 사용하여 데이터를 나타내며 HTML과 마찬가지로 데이터를 표현하는 방법 중의 하나이다. 서버와 데이터를 주고 받을 때에는 XML뿐만 아니라 다양한 파일 포맷을 통해 주고받을 수 있는데 요즘은 JSON을 많이 사용하는 추세이다.

### XHR의 탄생

AJAX와 XHR(XMLHttpRequset)이 활발히 개발되고 있을 당시 마이크로소프트사에 있는 아웃룩 개발팀이 활발히 참여하였는데 이 개발팀은 통신수단으로 XML을 사용하였기 때문에 전송방법(HttpRequest)의 앞에 앞에 XML을 뜻하는 X를 넣게 되었다. 데이터를 주고 받을 때에는 XML 뿐만 아니라 다양한 타입의 데이터를 주고받을 수 있기 때문에 앞에 XML을 붙인 것은 잘못된 명명법으로 간주된다. 함수나 클래스 등 외부로 노출되는 API를 만들 때에는 API의 이름을 명료하게 작성해야 하는점 잊지말자!

## JSON

브라우저에서 통신을 할때는 fetch()나 XHR오브젝트를 사용하여 통신을 하는데 XML을 사용하면 불필요한 태그가 너무 많이 들어가서 사이즈도 커지고 읽기 불편하기 때문에 요즘은 JSON을 많이 사용한다. JSON은 JavaScript Object Notaiton의 약자로 1999년 발표된 ECMAScript3의 오브젝트에 영감을 받아서 만들어진 데이터포맷이다. 자바스크립트에서의 오브젝트는 키(Key)와 값(Value)로 이루어져 있는데 브라우저 뿐만 아니라 모바일에서 서버로 데이터를 주고받을 때, 또는 서버와 통신을 하지 않더라도 오브젝트를 파이릿스템에 저장할때에도 JSON타입을 많이 사용하고 있다.

한마디로JSON은

- 데이터를 주고 받을 때 사용될 수 있는 가장 간단한 파일 포맷
- 텍스트 기반의 가벼움
- 사람이 읽기 편함
- 키와 값으로 이루어져 있는 파일 포맷
- 데이터를 서버와 주고 받을 때 직렬화(serialization)을 위해서 사용
- 프로그래밍언어나 플랫폼에상관 없이 쓰일 수 있다 `!important`

### JSON 공부 포인트

1. 오브젝트를 어떻게 직렬화하여 전송하는가?
2. 직렬화된 JSON을 어떻게 deserialize하여 데이터로 변환하는가?

JSON오브젝트 안에는 두개의 API가 있다. `parse` `stringify` 가 있는데 각 API는 오버라이딩 되어 있다.

### Object to JSON

```
// JSON오브젝트에 있는 stringify와 parse메서드를 사용하자.let json = JSON.stringify(true)console.log(json) // truejson = JSON.stringify(['apple', 'banana']);console.log(json) // ["apple", "banana"] -> 싱글에서 더블 쿼테이션으로 바뀐 거에 주목!const rabbit = {  name: 'tori',  color: 'white',  birthDate: new Date(),  jump: () => { console.log(`${name} can jump!`) }}json = JSON.stringify(rabbit)console.log(json) // {"name":"tori","color":"white","birthDate":"2020-10-30T08:59:05.130Z"}// jump라는 함수는 데이터에 포함되지 않는 것을 볼 수 있다!// JS에만 있는 Symbol과 같은 기능들도 JSON에 포함되지 않는다.// reviver를 사용하여 오브젝트를 정제할 수 있다.json = JSON.stringify(rabbit, ['name']) // {"name":"tori"}json = JSON.stringify(rabbit, ['name', 'color']) // {"name":"tori", "color":"white"}json = JSON.stringify(rabbit, (key, value) => {  console.log(`key: ${key}, value: ${value}`)}) // 제일 처음 전달되는건 오브젝트 그 자체! 그 후 오브젝트의 값들을 하나씩 읽는다.
```

### JSON to Object

```
json = JSON.stringify(rabbit);const obj = JSON.parse(json);console.log(obj) // {"name":"tori","color":"white","birthDate":"2020-10-30T08:59:05.130Z"}// 함수는 serialize될 때 JSON으로 변환되지 않았으므로 obj에는 원래 rabbit에 있던 jump()가 없다.rabbit.jump() // can jump!obj.jump() // Uncaught TypeError// 원래 rabbit에 있는 birthDate는 오브젝트이나 JSON으로 들어간 값은 스트링이므로 // Date에 관련된 API를 사용할 수는 없다.console.log(rabbit.birthDate.getDate()) // 2020-10-30T08:59:05.130Zconsole.log(obj.birthDate.getDate()) // Error!// 이는 reviver에 콜백함수를 넣어서 obj안에 Date 오브젝트를 넣어줄 수 있다const obj = JSON.parse(json, key, value) => {  return key === 'bitrhDate'? new Date(value): value;})
```

## 학습에 유용한 사이트

[JSON DIFF](http://www.jsondiff.com/) : 두개의 JSON을 비교하여 다른점을 찾아주는 사이트. 디버깅할때 유용하다.

[JSON Beautifier](https://jsonbeautifier.org/) : JSON 포맷을 복사하면서 포맷이 망가지는 경우 고쳐준다.

[JSON Parser](https://jsonparser.org/) : JSON 파일의 오브젝트 타입을 미리 만들어서 보여준다.

[JSON Validator](https://tools.learningcontainer.com/) : 해당 JSON 이 유효한지 검사를 해준다!