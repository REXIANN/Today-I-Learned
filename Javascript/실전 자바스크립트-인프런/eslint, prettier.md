# eslint, prettier

## eslint

정적 코드 분석기

```bash
npm install eslint

# 초기설정
npx eslint --init
# 이후 몇가지 옵션을 선택하면 된다
```

rules에서 사용할 플러그인을 세팅할 수 있다. 0은 꺼짐을 의미한다.

'off'로 해도 된다

## prettier

```bash
npm install prettier
```

vscode를 사용한다면 prettier extension을 설치해서 사용하는걸 추천함

파일을 저장할때마다 프리티어를 실행하도록 vscode의 formatOnSave를 지정

Default Formatter에 프리티어를 추가