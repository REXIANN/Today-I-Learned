# EAFP와 LBYL

쉽게 말해서 EAFP는 try ~ except, LBYL은 if문 스타일이다.

## EAFP

- It's Easy to Ask Forgiveness than Permission
- 허락을 구하는 것 보다 용서를 구하는 것이 쉽다
- 일단 수행시키고(try), 에러가 발생하면 그때 처리한다(Except)

## LBYL

- Look Before You Leap
- 누울 자리를 보고 발을 뻗어라
- 어떤 코드를 실행하기 전에 에러가 발생할만한 부분의 조건을 미리 따져 본다

파이썬에서는 LBYL보다 EAFP를 권장한다.[PEP-0463]([https://www.python.org/dev/peps/pep-0463/](https://www.python.org/dev/peps/pep-0463/))

둘의 차이는 무엇일까? 다음과 같은 경우를 생각해보자

```python
grade_dict = {
	'Lee': 'A0', 
	'Kim': 'B+',
	'Choi': 'C0',
	'Park': 'A+',
}

name = 'Lee'

# 'Lee' 학생의 성적을 알고 싶다.
```

이 경우 'Lee' 학생의 성적을 알기 위해서는 이렇게 하면 된다.

`grade_dict[name]`

그러나 해당 학생이 성적분포도에 없다면 문제가 될 수도 있다. `~~.get()` 은요? 쉿 조용~~

LBYL스타일의 경우 다음과 같이 작성한다.

```python
grade_dict = {
	'Lee': 'A0', 
	'Kim': 'B+',
	'Choi': 'C0',
	'Park': 'A+',
}

name = 'Lee'

if name in grade_dict:
	print(grade_dict[name])
```

별다른 문제가 없어 보일 수 있다.

하지만 if문에서 'Lee'가 grade_dict의 키 값이라는 것을 확인하고 다음줄로 넘어간 순간, 알 수 없는 이유로 grade_dict에서 'Lee' 키가 지워진다면 문제가 발생할 수 있다.

현재 코드에서는 그럴일이 없지만 비동기 상황에서는 충분히 벌어질 수 있는 일이다. 그럴 경우 예외처리를 해두지 않았기 때문에 그대로 프로그램이 뻗어버리는 불상사가 발생할 수 있다.

EAFP 스타일로 작성하면 다음과 같다.

```python
grade_dict = {
	'Lee': 'A0', 
	'Kim': 'B+',
	'Choi': 'C0',
	'Park': 'A+',
}

name = 'Lee'

try:
	print(grade_dict[name])
except KeyError:
	pass
```

우선 'Lee' 학생의 성적 정보를 출력하라고 하고, 에러가 생기면 그때 상황에 맞는 처리문을 동작시키는 방식이다.

예외처리를 해두었기 때문에 프로그램이 뻗어버리는 불상사가 발생하지 않는다(서비스가 유지된다)

이러한 이유로 파이썬은 EAFP 스타일의 코드를 추천한다.

딕셔너리의 키를 가져오는 상황에서 발생하는 KeyError뿐만 아니라 iterator의 다음 요소를 가져오는 상황에서 발생하는 StopIteration, 리스트나 튜플의 인덱스를 가져오는 상황에서 발생하는 IndexError 등 다음값을 가져오는 상황에서 발생하는 에러에 대한 대처를 except로 처리하는 방식을 사용하도록 하자.