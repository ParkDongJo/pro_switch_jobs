
# 뜻 풀이
Partial Pre / Rendering  부분적으로 클라이언트 요청 전에 / HTML+데이터를 보여주는 것(준비하는 것)

> 클라이언트 <---- ----> 서버 (HTML + 데이터 미리 만들어놓고)

Pre Rendering == Static Rendering 과 동일하다.


# 시대의 변화
웹 서비스가 고도화 되면서, 정적인 문서전달에서 동적 화면 관리가 필요하게 됨.

> Static Rendering --> Dynamic Rendering

Dynamic Rendering 을 지원하는 라이브러리 등장으로 React 가 그중 하나이다. 
하지만, 웹서비스에는 동적인 페이지 + 정적인 페이지 둘다 관리된다.  하지만 React 는 Dynamic Rendering 만 수행할 수 있다.

>  Dynamic Rendering --> 페이지 단위로 Rendering 방식을 선택 (Dynamic + Static)


# NextJS 의 변화
현재 구조로는 페이지 단위로 Dynamic/ Static 분리가 되게끔 구현 되어있다
NextJS 초기 버전
- getInitialProps
- getServerSideProps
- getStaticProps

NextJS 13v 이후로는
Dynamic Rendering 일 경우는
- app router
- Dynamic API
	- headers()
	- cookies()
	- fetch({ cache: 'no store' })
그 외의 경우는
- Static Rendering 으로 처리하기

지금까지 페이지 단위의
- HTTP 캐싱 정책
- route segment
으로도 충분했다.

**하지만,**
아래와 같이 변화가 요구되고 있다.

> 페이지 단위 Dynamic / Static 분리 ---> 컴포넌트 단위로 Dynamic / Static 분리


# 왜 이런 변화가 요구되는가?
현재 구조에서는 하위 컴포넌트 가 Dynamic 이라면, 전체가 Dynamic 페이지로 판단된다.

```
        S
       / \
      S   S
     / \   \
    S   D   D
```

왜 이렇게 구현했는가?
- 이렇게 해야 내부 구조가 심플해지기 때문,
- 컴포넌트 단위로 하는 것은 오버 엔지니어링 이였기 때문,
- 이정도로도 불필요한 순회를 막을 수 있었음
- React 에 발맞춰감

하지만 
React 17 버전 이후로 부터, SeverComponent, Suspense 등등의 큰 변화가 생기면서

- Suspense 비동기 작업이 필요한 컴포넌트의 경계를 지어주고 지연을 시켜줄 수 있도록 가능해짐
- SeverComponent 의 고도화로 인해, RSC/ RCC 구분이 이미 용이해졌고 이를 토대로 개선이 가능해짐


# 예상되는 변화
NextJS 의 내부에서 관리되는 workStore 상태의 구조적인 변화
현재는 workStore 의 forceDynamic 이라는 상태로 페이지 단위로 Dynamic 을 강제화 하고 있지만,

```typescript
const workStore = {
  boudaries: new Map([
    ['boundary-1', {
      mode: 'static',
      revalidate: false,
    }],
    ['boundary-2', {
      mode: 'dynamic',
      streaming: true,
    }]
  ]),
  getBoundaryConfig(boundaryId) {
    return this.boundaries.get(boundaryId)
  }
}
```

위처럼 workStore 구조에서 각 컴포넌트단위로 mode를 각각 나누고 각 모드와 속성에 따라 다르게 빌드되도록 관리되지 않을까? 강의자가 예상했었음


## NextJS 문서 참고자료
---
Partial Prerendering
https://nextjs.org/docs/app/getting-started/partial-prerendering

dynamic routes
https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes

Partial Prerendering 이미지로 설명 (좀 어려움...)
https://www.youtube.com/watch?v=MTcPrTIBkpA
![[Pasted image 20250921135620.png]]
![[Pasted image 20250921135754.png]]