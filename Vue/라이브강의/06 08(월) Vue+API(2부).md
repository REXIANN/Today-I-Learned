# 06/08(월) Vue+API(2부)

Created: 2020년 10월 4일 오후 11:41

cookie 안에 있는 session_id: 'asdf'

세션 인증은 기본적으로 템플릿, 즉 UI파트가 쪼개져나오면 쓸 수 없다. 그것을 보완하기 위해 token이라는 것을 만들어서 발행한다. 지금처럼 django와 vue가 분리가 된 상황에서는 다르다.

1. vue→django: 요청(회원가입 또는 로그인)
2. django → validation: 유효하다면 token과 user_id를 테이블에 저장.
3. django → vue: 응답: { token: adsf }
4. 이후 vue에서 django로 보낼 때 header에 { "Authorization": "Token adsf")을 넣어야 함. header에 해당 object가 있으면 자동으로 request.user에 유저정보가 들어감. 이를 도와주는 라이브러리는 `django-rest-auth` 와 `django-allauth`

로그아웃의 핵심은 Token Destory! (테이블에서 해당 토큰을 지워버림)

마지막에 JSON WEB TOKEN설명해주는거 다시듣고 적어놓기