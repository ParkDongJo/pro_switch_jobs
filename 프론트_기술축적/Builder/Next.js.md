
## # Learn Next.js 앱라우터 학습 코스

튜토리얼
https://nextjs.org/learn/dashboard-app






## Next.js 13 App router + React Query

https://xionwcfm.tistory.com/339

두가지 방법이 존재하는데

initialData 속성을 이용한 방법과
Hydration을 이용한 방법이 있습니다.

튜토리얼
[https://codevoweb.com/setup-react-query-in-nextjs-13-app-directory/](https://codevoweb.com/setup-react-query-in-nextjs-13-app-directory/)





Server Component 를 지원하는 라이브러리들
https://github.com/reactwg/server-components/discussions/6




## SSR 직접 구현
-----


Bun 과 리액트로 SSR 구현
https://velog.io/@eunbinn/server-side-rendering-ssr-with-bun-and-react

바닐라 스크립트로 SSR 직접 구현
https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Server-Side-Rendering/#reference




## SSR vs. CSR vs. ISR vs. SSG
----
웹 페이지를 렌더링 하는 카테고리는 크게 4가지로 나뉜다. 

### CSR 방식
서버에게서 리소스와 데이터들을 받으면, HTML, CSS, JavaScript 를 브라우저에서 동적으로 렌더링 하는 방식이다.

장점
- 모든 스크립트가 브라우저에 로드 되었기 때문에, 후속 렌더링이 빠름
- 서버 렌더링을 할 필요가 없어 적은 자원으로 서비스 제공이 가능하다.
단점
- SEO 에 친화적이지 않다.
- 초기 렌더링이 SSR에 비해 느리다.

### SSR 방식
첫 요청 시 웹서버에서 리소스와 데이터를 조합하여 HTML 파일을 생성한 후 브라우저에 전송되는 방식이다. 덕분에 초기 랜더링 속도가 빠르다.

장점
- SEO 문제 해결
- 초기 렌더링이 빠름
- 서버에서 페이지 관련 로직이 실행되면, 브라우저 단에서는 TTI을 빠르게 수행할 수 있다.
단점
- 페이지 이동 마다 서버 렌더링을 해야한다.
- 캐싱을 적용하기 힘들다.
- 서버 부하 시 느려질 수 있다.

### SSG 방식
빌드 단계에서 각 페이지에 대한 HTML 파일을 생성한 후 브라우저로 전송되는 방식이다.

장점
- SEO 문제 해결
- 정적 파일에다가 캐싱을 활용함으로 매우 빠름
단점
- 동적 데이터를 표시하지 못함
- 내용 수정 시 재빌드, 재배포를 해야함

### ISR 방식 (= SSG + SSR)
빌드 단계에서 페이지에 대한 HTML 파일을 생성한 후 브라우저로 전송되고, 이후 특정 간격으로 페이지가 재생성 된다. 이는 SSG 와 SSR를 결합한 방식이다. 하지만, 이를 제공하는 프레임워크마다 방식이 약간씩 다른것 같다.

장점
- SEO 문제 해결
- 정적 파일에다가 캐싱을 활용함으로 매우 빠름
- SSG 의 속도를 가져가면서, 동적 데이터를 표시를 하지만 매 요청마다 필요없는 경우에 유리
단점
- 실시간 데이터 업데이트에는 적합하지 않다.
- 데이터 최신화가 일관되지 않을 수 있다.

## Universal Rendering 방식 (= SSR + CSR)
첫 요청시에 SSR을 수행하고, 그 이후로는 CSR 을 수행하는 방식이다.

장점
- SEO 와 초기 렌더링 문제 해결
- 초기 렌더링 이후 CSR 수행을 통해, 클라이언트 서버 비용 최소화
- CSR 수행 시 빠른 페이지 이동 + 동적 렌더 속도 향상

단점
- 서버와 클라이언트 양쪽 모두를 신경쓰는 코드를 관리해야한다.
- 초기 렌더링 시 서버 부하가 있으면 느려질 수 있다.


https://ajdkfl6445.gitbook.io/study/web/csr-vs-ssr-vs-ssg#universal-rendering
https://www.educative.io/answers/ssr-vs-csr-vs-isr-vs-ssg



## 하조은의 Next.js 13
---
파트 1
- 서버 컴포넌트
- 라우팅
- 페이지 간 이동
- 스타일링
- 데이터 패칭
- 메타데이터





## Turbopack 이란
-----
터보팩에 대한 공홈 읽어보기

https://turbo.build/pack/docs/why-turbopack
turbopack 은 rust 기반으로 만들어졌다. 덕분에 빠르고 병렬처리가 가능하다.
일단 공홈에서 내세우는 turbopack 의 장점으로는 아래와 같은 것들이 있다.

- Next.js 를 통해 SSR 구축을 손쉽게 이용할 수 있다.
- Native ESM 은 대규모 프로젝트에서 모듈단위 요청으로 인해 네트워크 요청이 급증하는걸 단점으로 찍고 있다. Turbopack 은 하나의 번들링을 여전히 고수하면서도 빠른 속도를 자랑하고 있다.
- 캐시와 병렬처리를 통해 속도를 증가시켰다
- 요청 기반으로 번들링한다.

https://turbo.build/pack/docs/core-concepts
코어에 대한 내용도 재미있다. 코어 내용으로는 아래와 같은 내용들이 기재 되어 있었다.

- 함수 레벨의 캐싱을 통해서 re bundling 할때 시간을 절약할 수 있다.
- 요청 기반의 컴파일링
	- Next.js 11 이전에는 전체 파일을 번들링했고
	- Next.js 11 에서 부터는 페이지 단위로 번들링했다.
		- 하지만 이는 페이지에서 의존하고 있는 모든 파일을 불러온다
	- Next.js 13 이후로 요청 하는 파일에 대해서만 컴파일 한다.
		- 화면에 표시 될때까지 컴파일을 하지 않는다.
		- 심지어 크롬 Dev tools 를 열고 있지 않으면, 소스맵 컴파일도 하지 않는다
		- ESM 을 사용해도 비슷한 동작이 발생하지만, 서버에 많은 요청이 가게된다.
		- 요청 수준의 컴파일을 사용하면 요청 수를 줄이고 기본 속도를 유지하면서 컴파일 할수 있다.

요청 기반의 컴파일 개념이 매우 궁금해졌다. 도대체 어떻게 요청 기반으로 한다느 걸까?
GPT 한테 일단 물어봤지만.. 일단 내가 직접 찾아보긴 해야한다

![[turbopack_gpt.png]]



https://careerly.co.kr/comments/71298



## 싫다 vs. 좋다
-----

싫다는 의견
https://www.epicweb.dev/why-i-wont-use-nextjs


좋다는 의견
https://arc.net/l/quote/nhgohjhn