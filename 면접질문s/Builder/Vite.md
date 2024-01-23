
## vite 에 대해
----
Webpack 과 같은 전통 번들러들은 애플리케이션의 규모가 커지면서, 빌드 시간까지 길어지는 시점에 오게 되었다.

특히 개발 시 작은 변경에도 다시 번들러를 돌려야하는 상황도 발생한다.


vite 는 이런 지점을 해결하고자 나온 차세대 번들러중에 하나이다. 
똑똑하게도 문제를 구분한다.

#### 개발 환경
먼저 개발 시에는
vite 는 어플리케이션 모듈을 2가지로 분류하고 이를 각기 다른 방식으로 번들링 한다.

- Dependencies
	- 개발 시 내용이 바뀌지 않을 내용들에 대한 소스코드이다
	- 사전 번들링을 통해 빌드 속도를 개선한다.
	- Go로 작성된 Esbuild를 통해 빌드 함으로, 매우 빠르다.
- Source code
	- JSX, CSS 등과 같이 컴파일링이 필요하고 수정이 잦은 소스코드이다.
	- Native ESM을 이용하여, vite 는 수정된 모듈이 있으 ㄹ시 그 모듈과 관련된 부분만 교체하고 브라우저가 요청 시 전달하기만 한다.
	- 브라우저는 필요한 모듈만 요청해서 받은 후 화면에 보여준다.

#### 프로덕션 환경
프로덕션 환경에서는 번들링을 한다. ESM 이 지원된다 하더라도 여전히 여러 모듈을 매번 네트워크 한다는 것은 비용이다. 
결국 프로덕션 환경에서는 하나의 파일로 번들링 하면서,

- 트리쉐이킹을 하여 사용하지 않는 코드를 제거하고
- 지연로딩, 청크 분할을 통해 로딩 성능을 최적화

하는 것이 더 바람직 하다

또한 프로덕션에서는 Esbuild 를 사용하지 않는데, 유연한 플러그인 API 사용을 위해 Rollup을 채택한 것으로 보인다. Rollup 역시 Webpack 보다 빠른 성능을 보이는 번들러이며, Rust 포팅을 준비하고 있는 Rolldown 기술 적용을 기대하고 있는 것으로 보인다.



## vite 사용법
----


### plugin 사용법
- react 사용시 -> vite-plugin-svgr
- svg 사용시 -> @vitejs/plugin-react


### 설정법
- Console 제거
	- terser 설치
```javascript
export default defineConfig({
    // terser을 활용하여 console 제거 설정
    build: {
        minify: 'terser',
        terserOptions: {
            compress: {
                drop_console: true,
                drop_debugger: true,
            },
        },
    },
});
```
- Proxy 설정
	- 프론트에서 CORS 에러를 해결
	- builder 에 proxy 설정
```javascript
export default defineConfig({
    // proxy 설정
    server: {
        proxy: {
            // /api/getData → http://localhost:8080/getData로 변경
            '/api': {
                target: 'http://localhost:8080', // fetch 요청에 대한 target 경로 설정
                changeOrigin: true,
                rewrite: (path) => path.replace(/^\/api/, ''), // /api에 해당하는 경로를 삭제
            },
        },
    },
});
```
- 경로 Alias 설정
	- 경로에 대한 alias 를 설정하여, 경로 설정을 좀 더 편하게 할수있도록 셋팅
```javascript
import path from 'path';

export default defineConfig({
    resolve: {
        // /src경로를 @로 alias 처리
        alias: [{ 
	        find: '@', 
	        replacement: path.resolve(__dirname, 'src') 
	    }],
    },
});
```
또는
```javascript
export default defineConfig({
    resolve: {
		alias: {
			'@root': '/',
			'@': '/src',
		},
	},
});
```
- 코드 스플리팅
	- 코드 스플리팅을 적용하여 소스 분리를 할 수 있다.
```javascript
import { splitVendorChunkPlugin } from 'vite';

export default defineConfig({
    plugins: [
        splitVendorChunkPlugin(), // vendor code spliting 설정
});
```

```javascript
import { lazy } from 'react';
import { Route, Routes } from 'react-router';

const Home = lazy(() => import('@/pages/home')); // lazy를 이용한 코드 스플리팅 설정
const Setting = lazy(() => import('@/pages/setting')); // lazy를 이용한 코드 스플리팅 설정

const App = () => {
    return (
        <Routes>
            <Route path="/home" element={<Home />} />
            <Route path="/setting" element={<Setting />} />
        </Routes>
    );
};

export default App;
```


webpack proxy 설정
https://velog.io/@jjhstoday/webpack-proxy%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-CORS-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0-%ED%95%98%EA%B8%B0
vite 기본 사용법
https://jforj.tistory.com/343



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



## Vite vs. Next(turbopack)
----
터보팩이 정말 vite 보다 10배 빠른가? 에 대한 의문점을 제시하는 글이다.

여기서는 vercel 에서 제공하는 벤치마크가 공정한 테스트가 아니라고 주장하고 있다.
추후에 시간 날때 정리해보자

https://velog.io/@arthur/%EB%B2%88%EC%97%AD-%ED%84%B0%EB%B3%B4%ED%8C%A9%EC%9D%80-%EC%A0%95%EB%A7%90%EB%A1%9C-Vite-%EB%B3%B4%EB%8B%A4-10%EB%B0%B0%EB%8A%94-%EB%8D%94-%EB%B9%A0%EB%A5%BC%EA%B9%8C-Is-Turbopack-really-10x-Faster-than-Vite




## vite 에 styleX 설정
----
vite-plugin-stylex 를 설치하여, config 에 설정

https://arc.net/l/quote/ppfgdlkw
https://medium.com/@huseyinsalmansoftdev/react-stylex-vite-npm-db9be1e5c5c6


vitest jest파일에 전역 jest 객체 사용
https://arc.net/l/quote/uvgtkfqw
https://github.com/vitest-dev/vitest/issues/2667




## vite 가 탄생하기 까지
-----

https://velog.io/@teo/vite?trk=public_post-text