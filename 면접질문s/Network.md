

## HTTP 와 HTTP2 의 차이점
------



## HTTP2 와  HTTP3 의 차이점
-------



## JWT
------
쿠키, 세션 그리고 JWT

JWT는 토큰을 활용한 인증 방식이다. JSON WEB TOKEN 이라는 이름에서도 알수 있듯이 JSON 객체에 인증에 필요한 정보들을 담은 후 비밀키(시크릿 키)로 정보들을 암호화 하여 전송한다.

JWT의 사용은 아래 순서로 이뤄진다.
- 클라가 로그인 요청
- 서버는 비밀키를 이용해서 JWT 토큰 발급
- 헤더에 JWT를 담아 클라에 보냄
- 클라가 JWT를 저장해둠
- API 호출을 할때마다 header에 JWT를 실어 보냄
- 서버는 매번 헤더를 확인 하여, 체크 후 API 응답을 보냄


JWT 의 구조는 아래와 같다.

|header|payload|signature|
|------|---|---|
|xxxxx|yyyyyy|zzzzz|


- header 는
	- 사용한 알고리즘 : RS256, HS256
	- 토큰 타입 : json
- Payload
	- sub : 토큰 제목
	- aud : 토큰 대상자
	- iat : 토큰이 발급된 시각
	- exp : 토큰의 만료 시각
- Signature
	- header 와 payload를 합친 문자열을 header 에서 기재된 방법으로 암호화 한 값
	- 서버에서 signature를  비밀키로 푼 후에  payload와 signature 와 동일한지 대조 해봄


## OAuth
-----





## React Query