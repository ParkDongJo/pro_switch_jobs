

## babel 은 어떻게 ES6를 ES5로 변환시켜주나
----






## SWC
----
Rust 로 제작된 JavaScript 트렌스파일러이다. Babel 과 비교 시 변환 속도에서 큰 차이가 난다. 그와 동시에 Webpack 과 같은 번들러의 기능도 확장할 수 있다.

Rust 라는 프로그래밍 언어는 병렬 처리를 고려해서 설계된 언어인데, 싱글쓰레드 기반인 JavaScript 로 제작된 Babel 비해 빠를 수 밖에 없다.

덕분에 SWC 는 의존성이 없는 파일들을 동시에 변환할 수 있다. 하지만 꼭 병렬처리가 아니더라도 SWC는 바벨보다 훨씬 빠르다는 벤치마크는 쉽게 찾아볼 수 있다.

이 때문에, Next.js 도 12v 부터 기존에 babel과 terser 이 하던 역할을 SWC 로 모두 교체했다.

https://fe-developers.kakaoent.com/2022/220217-learn-babel-terser-swc/