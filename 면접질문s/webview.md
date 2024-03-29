

https://www.youtube.com/watch?v=hsh8BS7gyrY


웹
- 네이티브 앱 보다 생산 비용이 저렴
- 상시 배포 가능

앱
- 다양한 모바일 기기의 기능과 호환
- 앱스토어 심사 절차를 거쳐야한다. -> 강제업데이트

웹뷰
앱이 웹 컨텐츠를 표시하는데 사용할 수 있는 내장형 브라우저

![[webview_1.png]]


웹뷰 통신 객체

window.postMessage()
window.addEventListner()

https://www.youtube.com/watch?v=LbU5E1pWfks

웹뷰가 좋은 이유
- 웹과 앱을 한번에 개발 가능
- 앱스토어 심사가 필요 없음 (단 native 코드의 변화가 없을 시)
- 런타임 시 발생하는 이슈 즉각 대응 가능
- 특정 플랫폼에 종속되지 않음
- 네이티브 기능을 함께 쓸수 있다

웹뷰가 안좋은 이유
- 성능이 네이티브보다는 안좋다
- 보안 이슈
- 사용자 경험이 네이티브보다 매끄럽지 못하다
	- 계속 업데이트 되는 앱 UX를 사용할 수 없는 경우가 발생한다.

웹엔진
- webkit (애플) / chromium(webkit을 fork한 blink 기반) (구글)
- 폴더 구조로 알아보는 webkit ![[webview_2.png]]
- 폴더 구조로 알아보는 chromium![[webview_3.png]]


FE 개발자들의 어려움
- 아이폰에서 인스팩터가 안돼요
- 개발자 계정에 등록된 기기들만 된다 -> 기기등록 제한

- iOS 는 16.4 부터 
	- 등록되지 않은 기기에 인스펙터를 설치해서, 띄울 수 있다.
	- 앱을 맥북에 설치해서, 인스팩터로 확인 할 수 있다.

- 사파리, 크롬에서 된다해도 -> 같은 웹엔진을 쓴다해도 웹뷰 내에 내장된 웹엔진에서는 다를 수 있다.
	- 앱과 웹이 서로 통신할때는 primit (원시) 타입들만 주고 받을 수 있다.


APP 개발자들을 위한 레시피
- iOS 네이티브
	- 멀티 프로세스 - 비동기 API 이슈
	- 사파리와 웹뷰는 다르다 (SFSafariViewController 로 그냥 보여지기만 할때 활용)
	- 앱이냐 웹이냐 문제 지점을 빠르게 판단해야함
- 쿠키 같은 로컬 데이터 동기화
	- 앱이 쓰면 -> 웹에 동기화
	- 웹이 쓰면 -> 앱에 동기화

앱개발자에 웹뷰에서 무언가 하기위해서
- 웹 코드 호출이 어려울때,
	- try - catch() 로 묶지 마라 -> 이슈 분석이 어려워짐
	- 아래와 같이 대응![[webview_4.png]]

- 자바스크립트 오버라이딩
	- 웹로그를 네이티브로 가져오기
		- 앱개발자가 웹개발자의 로그를 앱에서 확인 가능
	- 히스토리 관리
		- 히스토리 이벤트 넘겨서, 화면 이동 앱에서 관리
	- 아래와 같이 구현 ![[webview_5.png]]


앱 스킴이 호출되지 않는 이슈

앱에서 웹을 호출 시 아래 문자열 조심
	- ''
	- ""
	- \
	- 해결은 Encoding 또는 Escaping
		- Encoding 룰도 다른 경우가 있음
		- white space, + 가 Encoding이 안될 수도 있음

웹페이지가 하얗게 나올 때
- 임의의 웹페이지 방문 시 페이지가 뜨지 않을 때
- 각 플랫폼 별
	- iOS -> webViewWebContentProcessDidTerminate(:)
	- Android -> onRenderProcessGone()
- 일단 reload() 시켜야함
	- reload() 횟수를 제한
	- 제한 도달 시 에러 UI & 다시시도 버튼

웹페이지가 안나올 때
- 재미난 사례 -> RFC 6265
	- 웹 쿠키 제한 (모바일 4K)
	- 해당 사용자의 쿠키가 계속해서 쌓일 때
		- 배민에서는 운좋게 사내에서 1사람이 해당 이슈가 발견되었고
		- 해당 기기의 앱에 덤프 데이터를 가져와서, 분석을 하던 중 찾은 내용 중 1개
		- 쿠키가 용량 제한을 넘어갔음
			- 웹페이지 사용시 쿠키 제한을 신경써야할 것
			- 앱에서는 쿠키가 용량이 찼을 때, 해당 쿠키 삭제 후 다시 쿠키 셋팅

캐시 안했는데 
- 재미난 사례 -> RFC7234 (휴리스틱 캐시)
	- 캐시 사용하지 않았는데,
		- A 디바이스에서는 새페이지가 보이고
		- B 디바이스에서는 구페이지가 보였다.
	- 휴리스틱 캐시
		- 웹엔진에서 임의의 캐시를 진행
		- 해당 웹데이터의 10분의 1만 캐싱하도록
	- 이 페이지의 배포는 최소 2주전에 진행되어야 함을 계산해서 진행![[webview_6.png]]
	- 캐시를 안한다 하더라도, 캐시 정책를 비워두지 않고 아래와 같은 셋팅을 꼭 확인하고 한다.
		- no-cache
		- must revalidate
		- max-age : 0

앱과 웹이 통신하는 방법
- AppScheme
	- [sheme]://[host]/[path]?[query]
	- 2000~3000자의 제한
	- URL 로딩 시도
- JavaScript Interface
	-  앱과 사전에 정의한 자바스크립트를 호출하는 모던한 방법
	- 앱스킴과 다르게 호출 길이 제한이 없음
	- URL 로딩을 시도하지 않음


웹에서 앱으로 스크립트 호출
- 콜벡함수를 사전에 정의하지 않고 동적으로 받을 수 있도록 정의했을 시
	- 보안이슈 - XSS 조심해야함
	- 동적 호출 제거 -> 사전 정의후 호출로 변경![[webview_7.png]]

앱에서 웹으로 스크립트 호출
- 앱-웹이 함수 인터페이스를 맞춘 경우
	- 앱은 웹 함수 호출에 대한 예외 처리가 필요
	- 웹은 전역 함수 관리에 대한 유지보수 증가
		- 함수가 더이상 사용하지 않게 되더라도 앱 하위버전에 대한 대응으로 계속 남겨둬야함
- dispatchEvent() 로 통신
	- 앱은 함수 호출에 대한 예외처리 불필요
	- 웹은 eventListner 사용으로 호출
	- ![[webview_8.png]]


앱에서 바로 로그 확인
- 앱로그 + 웹로그
- https://github.com/kean/Pulse
	- 앱에서 확인 가능
	- mac 에서도 확인 가능
- https://github.com/liriliri/eruda





## 웹뷰에서 마주할 수 있는 이슈들
----



https://shylog.com/settings-for-a-more-complete-webview/




## 웹뷰의 장단점
----



https://velog.io/@bangina/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98-Webview-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A0%88%EC%8A%A8%EB%9F%B0-%EC%A0%95%EB%A6%AC