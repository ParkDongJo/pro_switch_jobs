

## jest 셋팅
-----

https://velog.io/@thdrldud369/Vite-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-jest-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0

## router path 정의
-----


https://arc.net/l/quote/tlujvsno


## 추가 설정이 필요한 plugin
----

SVG
https://github.com/pd4d10/vite-plugin-svgr
- ts 에서 사용하려면 추가설정을 해야한단다. https://ddd120.tistory.com/48
- 음.. 근데 실제로 적용해보면 추가설정은 필요 없다.




## vite 와 jest
----

vite 는 ES6 기반이고
jest 는 commonJS 기반이다

jest 대신 vitest 로
https://codingpr.com/test-your-react-app-with-vitest-and-react-testing-library/

vite 를 사용한다면, vitest 를 쓰는것이 좋다. jest 의 대부분 API 들이 호환이 된다.
그럼에도 jest 를 쓰고 싶다면,
https://arc.net/l/quote/jmjrorcw

추가적인 여러 설정들을 해야한다.
이 글이 jest 사용을 위해 여러 설정을 하는 걸로 보이는데, 솔직히... 이럴거면 왜 vite 쓰나 싶을정도로 .. 따라하고 싶지 않다.
https://medium.com/@Jun_0/react-jest-dac7a8c83910




## vite 와 babel -> SWC
----
vite 에서는 babel 대신 swc 를 추천하고 있다.

```
개발 중에는 Babel 대신 SWC를 사용합니다. 빌드 중 플러그인을 사용하게 된다면 SWC+esbuild를 사용하고, 그렇지 않다면 esbuild만을 사용합니다.
```

라고 공식 홈페이지에 되어 있다. SWC 가 정확히 어떤 장점이 있고 어떤 용도일지는 좀 더 알아봐야한다.
babel 과 비교되는걸 보면, 성능이 좋은 트렌스 파일러이다.

실제로 속도를 비교해놓은 사이트가 있는데, 차이가 꽤 크다
https://swc.rs/blog/perf-swc-vs-babel

일단 빠르게 파악한 바로는 설정 방법은 간단하다
https://vitejs.dev/plugins/

공식 플러그인으로 등록 되어 있다.