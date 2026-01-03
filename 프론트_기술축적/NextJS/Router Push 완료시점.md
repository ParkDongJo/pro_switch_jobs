
- 완료 시점이 필요한 상황
	- Parelel Rote modal
- 완료시점 알수 있는 방법 구현
	- router.push 는 동기적으로 동작하지 않는 이유?
	- startTransition 으로 감싼 navigation 이다 (next.js 의 내부 코드를 보면)
		- 동시성 모드 지원
		- 업데이트 순서 관리 (Fiber 아키텍처와 관련있음)
		- 라우팅이 인터셉트 중간 중단
- Router.push 는 왜??? 비동기 처럼 동작하게 변경되었는가?
	- React 의 동시성 모드와 통합하기 위해서임!!!!

- useTransition() hooks 을 활용한다.  router.push 하는 영역을 startTransition으로 감싼다.
- usePathname() hooks 을 활용한다. pathname 을 트래킹한다.

useEffect 

isPending  False --> True --> False

pathname 현재 페이지 --> 현재 페이지 --> 타겟 페이지

Router.push 실행전   -->  실행시작 --> 실행완료






