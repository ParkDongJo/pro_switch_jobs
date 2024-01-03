

## local vs. session vs. cookie
----
브라우저의 저장소는 아래와 같이 크게 3가지 종류가 있다. 각각 고유의 특성을 가지고 있으며 이를 잘 활용해서 적절한 경우에 사용하는 것이 중요하다
#### 스토리지
##### 로컬
- key : value
- 최대 용량
	- 5~10MB
- 만료 조건 :
	- 강제 삭제 / 초기화 (=강력리셋)
- 공유 조건
	- 동일한 도메인
- 사용 예시
	- 자동 로그인
	- 사용자 맞춤 설정

##### 세션
- key : value
- 최대 용량
	- 5~10MB
- 만료 조건 :
	- 탭 종료
	- 브라우저 종료
	- OS 종료
 - 공유 조건
	 - 동일한 탭
	 - 동일한 브라우저
	 - 동일한 도메인
- 사용 예시
	- 예약 시스템 (단계별 저장)
	- 회원가입 유도 팝업

#### 쿠키
- cookie-name : cookie-value
- 최대 용량 :
	- 4KB
- 만료 조건 :
	- 만료기간
- 공유 조건
- 서버 전송 여부
	- 첫 응답 - Set-Cookie
	- 매 요청 - Cookie
- 사용 예시
	- 오늘 하루 보지 않기
	- 장바구니에 담기


#### 공통점
- 데이터는 문자열 형태만 가진다.


https://www.xenonstack.com/insights/local-vs-session-storage-vs-cookie
https://adjh54.tistory.com/57




## Worker 로 병렬처리
----
