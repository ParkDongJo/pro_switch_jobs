
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
- HTTP/2 버전
- 웹이 복잡해지면서 HTTP/1.1 의 한계점이 부각
	- 매 요청-응답마다 중복되는 헤더 전송
	- Head-of-Line Blocking 문제
		- 요청이 매번 순차적으로 처리
		- 중간에 요청/응답이 문제 발생 시, 후속 요청들은 대기해야함
- HTTP/2가 해결
	- SPDY 프로토콜 기반(구글에서 개발)
	 ![[HTTP_1.png]]
		- 이진(binary) 프로토콜
		![[HTTP_2.png]]
			- HTTP/1.1 은 텍스트 기반으로 메시지 전송
			- HTTP/2 바이너리 프레이밍을 통해, 이진 데이터로 전송
		- 응답 다중화 지원
		![[HTTP_3.png]]
		![[HTTP_4.png]]
			- 하나의 TCP 연결을 더 작은 단위로 잘게 세분화 (스트림, 메시지, 프레임)
			- 하나의 연결에 여러 스트림이 순서와 상관없이 전송
				- 클라에서는 이걸 다시 조립해서 사용
		- 헤더 필드 압축
		![[HTTP_6.png]]
			- HTTP/1.1 에서는 비대해진 헤더를 압축해서 사용
			- 달라진 부분만 다시 전송하는 허프만 코딩 사용


2019 ~ 2022년
- HTTP/3 발표
- HTTP/2는 결국 TCP 위에서 돌아감
	- HOLB 문제를 어느정도 개선은 했어도 해결을 하지는 못함
- UDP 기반으로 된 QUIC 라는 프로토콜 기반
	- UDP 가 보증하지 못하는 신뢰성을 구축하기 위해,
		- 패킷재전송, 혼잡제어, 흐름제어 기능을 직접 구현함
- 연결 정보를 캐싱하여 재사용할 수 있는 0-RTT 기능 제공
- 연결 다중화
	- 각 스트림이 독립적으로 동작
	- 데이터 손실이 발생해도 다른 스트림에 영향을 주지 않음
- IP 기반이 아닌 UUID를 이용해 연결
	- Wi-Fi 환경에서 셀룰러 환경으로 이동하는 경우 IP 주소 변경으로 연결 재수립함
	- 그에 반해 QUIC 는 UUID 기반으로 연결이 유지됨
- 헤더 압축이 더 개선됨 (QPACK)



https://yozm.wishket.com/magazine/detail/1686/
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


## RESTful vs. GraphQL
-----


## HTTP/1.1 와 HTTP2 의 차이점
------
HTTP/2는 웹이 복잡해지면서 HTTP/1.1 가 겪는 한계점을 해결하고자 나왔다. HTTP/1.1의 한계점은

- 매 요청-응답마다 중복되는 헤더 전송
- Head-of-Line Blocking 문제

이 있었는데, HTTP/2 는

- SPDY 프로토콜 기반으로 만들어 졌는데
	- 이진(binary) 프로토콜
	- 응답 다중화
	- 헤더 압축

등등을 기술적으로 보완하였다.


## HTTP2 와  HTTP3 의 차이점
-------
HTTP/2 는 기반 자체가 TCP 라 TCP의 한계점을 벗어나지 못했다. 앞서 이야기 되었던 Head-of-Line Blocking 문제는 완벽하게 해결하지 못했다.

TCP는 결국 신뢰성을 지향하기 때문에 데이터 손실이 발생하면 재전송을 우선 수행하기 때문이다. 프로토콜 자체의 불필요한 헤더도 한 몫 했다.

이를 해결하기 위해 HTTP/3 은 UDP 기반인 QUIC 프로토콜 위에서 동작한다.

- UDP 기반이기 때문에
	- 연결 정보를 캐싱하여 재사용할 수 있는 0-RTT  기능 제공
		- TCP 의 경우 최초 연결 수립 시 3-way 핸드 쉐이크 과정이 필요하지만, UDP는 최초 연결 설정에서 연결에 필요한 모든 데이터를 함께 전송함
		- 성공한 연결은 캐싱해놨다가 재사용
	- 연결 다중화
		- 여러 스트림이 독립적으로 동작
		- 데이터 손실이 다른 스트림에 영향을 주지 않음
	- 연결별 고유 UUID로 연결
	- 헤더 개선

등등이 해결 되었다. 하지만 HTTP/3의 채택은 아직 초기 단계이기 때문에 우선은 지속적인 관심으로 상황을 바라보는게 좋겠다.



## 인증과 인가
----



## 쿠키와 세션
----




https://www.daleseo.com/http-cookies/
https://www.daleseo.com/http-session/




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



## 쿠키 - 세션 vs. JWT
----





## OAuth
-----




## TCP 송수신 때 일어나는 일
----



https://youtu.be/K9L9YZhEjC0?si=m18rqvCUuSWBz5Ye



## CORS
----



https://www.youtube.com/watch?v=bW31xiNB8Nc