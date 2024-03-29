


https://blog.imqa.io/testing-framework-jest/
Jest 에 대한 고민



Jest 성능 개선
https://kyungyeon.dev/posts/102


## Jest 의 장단점
----
아래 내용을 내것으로 가공해야함
### Jest의 주요 특징

1. **Zero-Configuration**: Jest는 설정이 거의 필요 없는 '제로 설정' 철학을 가지고 있어, 복잡한 설정 없이도 쉽게 테스트를 시작할 수 있습니다.
2. **스냅샷 테스팅**: UI 컴포넌트의 렌더링 결과를 스냅샷으로 저장하고, 변경사항이 있을 때 이를 감지하는 기능을 제공합니다.
3. **테스트 커버리지**: Jest는 테스트 커버리지를 쉽게 생성하고 관리할 수 있는 내장 도구를 제공합니다.
4. **Mocking 및 Isolation**: Jest는 테스트를 위한 모의(mocking) 기능과 테스트 격리를 지원하여, 각 테스트가 독립적으로 실행될 수 있도록 합니다.

### Jest 사용의 장점

- **간편한 설정과 사용**: Jest는 설치와 사용이 간편하며, 많은 설정이 필요하지 않습니다.
- **빠른 실행 속도**: Jest는 테스트 실행 속도가 빠르며, 테스트 케이스 간의 병렬 처리를 지원합니다.
- **통합된 환경**: Jest는 테스트 실행, 모킹, 커버리지 분석 등 다양한 기능을 하나의 패키지로 제공합니다.
- **커뮤니티와 생태계**: Facebook과 많은 오픈 소스 커뮤니티의 지원으로, 풍부한 자료와 플러그인이 제공됩니다.

### Jest 사용의 단점

- **특정 유형의 테스트에 한계**: 대규모 프로젝트나 특정 유형의 복잡한 테스트에는 Jest만으로 충분하지 않을 수 있습니다.
- **통합 테스트와 E2E 테스트에는 제한적**: Jest는 주로 단위 테스트에 적합하며, 종단 간 테스트에는 Cypress나 Selenium 같은 도구가 더 적합할 수 있습니다.




https://voidx.net/jest/



## Jest 에서 SVG 테스트
----
SVG 가 적용된 component 를 모듈단위 테스팅 하고자 한다면

https://arc.net/l/quote/ovvtnqrp




## jest Vs. vitest
-----
jest 는 react 에서 개발한 test runner
vitest 는 vite 에서 개발한 test runner

vitest 는 jest 에서 제공해주는 API 를 제공해준다.


https://saucelabs.com/resources/blog/vitest-vs-jest-comparison



vite.js 에서 jest 를 설정해보고자 했지만, 설정 글마다 설정이 제각각이다!! 

https://jforj.tistory.com/362
위 링크는 jest 2023.11 설정하는 블로그임에도 불구하고 따라해봤자 전혀 실행이 안된다.
이딴 것에 시간 빼앗길 바에는 빡쳐서.. 됐다! 그냥 vitest 를 쓰는게 낫다.

현재까지는 vitest 를 사용했을 시 styleX 에 대한 지원이 아직 덜 된듯 하고, 관련 참고 자료도 적어서 styleX 를 안쓰고 tailwindCSS 를 쓰는 쪽으로 가고 있다.

