
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
