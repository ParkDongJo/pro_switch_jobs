

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




## vite 와 babel
----
vite 에서는 babel 대신 swc 를 추천하는