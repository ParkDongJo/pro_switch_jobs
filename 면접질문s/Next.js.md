
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

### SSR 방식
첫 요청 시 웹서버에서 리소스와 데이터를 조합하여 HTML 파일을 생성한 후 브라우저에 전송되는 방식이다. 덕분에 초기 랜더링 속도가 빠르다.

### SSG 타이밍
애초에 빌드 단계에서 각 페이지에 대한 HTML 파일을 생성한 후 브라우저로 전송되는 방식이다.

### ISR 방식
런타임 중에 정적페이지를 생성 또는 수정 할 수 있도록 해주는 SSG 와 SSR를 결합한 방식이다.


## Universal Rendering 방식

