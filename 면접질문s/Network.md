
## HTTP 의 변화
----
1990년대 초반
- HTTP/0.9 버전
- 초창기 웹은 서버 - 클라이언트 구조의 단순한 구조
- TCP/IP 위에서 구현되도록 설계 되었음
- GET 메서드


1996년
- HTTP/1.0 버전
- HEAD, POST 메서드 추가
- 상태 코드 추가
	- 200 / 300 / 400 / 500
- 헤더에 content-type 도입
	- MIME type - text/plain, text/html, application/json, application/xml, image/jpeg, image/png, image/gif


1997년
- HTTP/1.1 버전
- TCP 의 연결 상태 일정 시간동안 유지를 위한 Keep-Alive
- 요청이 있으면 이전 응답을 기다리지 않고 연달아 요청 보내는, 파이프라이닝 도입
- 헤더 기능 추가
	- Cache-Control 도입
	- 한 IP 에 여러 도메인 host 가 가능한 Host 도입
	- HTML 분할 전송
- PUT, OPTIONS 메서드 추가
- 쿠키 도입


1999년
- DELETE 메서드 추가


1994 ~ 2000년
- 2000년에 HTTPS 공식 규정
- TLS(SSL) 암호화 프로토콜 도입
	- 암호화 - 전송 중 제 3자가 해석 불가
	- 무결성 - 전송 중 변경 불가
	- 인증 - 주고받는 서버 인증
- 공개키 (클라에게 제공) - 개인키 (서버)
	- 공개키는 브라우저가 암호화 할때
	- 개인키는 서버가 복호화 할때


2000년
- RESTful API에 대해 박사 논문 발표
- 이후 AWS 같은 대기업이 채택
- 2016년 부터 MS가 REST API 에 대한 표준 가이드라인 제공
- XML, JSON 포멧 등장 + AJSX 기술 등장 -> RESTful API 설계가 각광받기 시작


2015년




https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design


## REST API 란
-----
서버와 서버간 통신의 경우 데이터만 주고받을 수 있는 API가 필요했는데, 이에 대한 표준이 없었다. 당시 HTTP가 웹 프로토콜의 표준이였고, 이를 효과적으로 사용하는 방향으로 의견이 모아졌다.

최초 제안은 한 대학원생의 박사논문으로 제안되었으며, 이를 계속 발전 시켜나갔다.

REST 를 만족시키는 3가지 구성요소는
- 자원을 표현하는 방법 - HTTP URI
	 - rooms/1/users/2
- 자원에 대한 행위 - HTTP Method
	- GET, POST, DELETE, PUT, PATCH (이를 CRUD 로 표현)
- 자원에 대한 내용 - HTTP Message Payload
```JSON
{
	"status" : "OK"
	"from": "localhost",
	"to": "https://hanamon.kr/users/1",
	"method": "GET",
	"data":{ "username" : "하나몬" }
}
```

로 정리할 수 있다. 이렇게 HTTP 의 특성을 적극 활용하여 서버의 자원을 표현하고 행위를 정의해서 데이터를 주고받는 설계를 RESTful API 설계라고 부른다.

##### RESTful API 특징
1. Server-Client(서버-클라이언트 구조)
2. Stateless(무상태)
3. Cacheable(캐시 처리 가능)
4. Layered System(계층화)
5. Uniform Interface(인터페이스 일관성)



https://arc.net/l/quote/siogunhf
https://jiwondev.tistory.com/266


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




## TCP 송수신 때 일어나는 일
----




https://youtu.be/K9L9YZhEjC0?si=m18rqvCUuSWBz5Ye