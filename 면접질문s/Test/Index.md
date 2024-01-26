


## Test Runner
----
https://arc.net/l/quote/cffqumit
위 글의 저자의 의견 정리
### 일반적인 비교
- Mocha, Jest, Jasmine은 모두 인기 있는 프레임워크로, 활발한 커뮤니티와 오랜 개발 기간을 가지고 있습니다.
- Mocha와 Jasmine은 Node 애플리케이션용으로 처음 구축되었기 때문에 백엔드 테스팅에 더 강점을 보입니다. 이들은 Jest보다 더 많은 백엔드 도구와 문서를 제공합니다.
- 프론트엔드 테스팅 프레임워크 선택은 주로 사용하는 프론트엔드 프레임워크에 영향을 받습니다. 예를 들어, Jasmine은 Angular와 함께 자주 사용되며, Jest는 React와 함께 사용하기 위해 Facebook에 의해 개발되었습니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.

### 비교 포인트
1. **함수 Mocking**:
    - 함수 호출은 애플리케이션에서 가장 일반적으로 테스트되는 부분입니다.
    - 함수 모킹의 경우 세 가지 라이브러리 모두 코드 라인과 복잡성이 매우 유사합니다. 자신의 스택에 가장 잘 맞는 라이브러리를 사용하는 것이 좋습니다: 
	    - React의 경우 Jest, 
	    - Angular의 경우 Jasmine, 
	    - 백엔드에서 Mocha를 사용하면서 일관성을 유지하려는 경우 Mocha를 사용하는 것이 좋습니다.
1. **데이터 Mocking**:
    - 백엔드 테스트는 Mocha와 Jasmine이 가장 강점을 보이는 분야입니다. 이 두 도구는 Node 애플리케이션을 테스트하기 위해 만들어졌으며, 도구에서 이를 확인할 수 있습니다. 프레임워크에 포함된 옵션과 기능을 통해 보다 세밀하게 제어할 수 있습니다. 사용 가능한 라이브러리 중 일부를 추가하는 데 시간을 할애할 의향이 있다면 Jest도 여전히 훌륭한 옵션이 될 수 있습니다.
2. **비동기 호출 Mocking**:
    - 백엔드 테스트의 경우, 비동기 메서드를 쉽고 즉시 처리할 수 있는 Jasmine을 가장 먼저 선택했습니다. Mocha와 Jest도 유용하지만, 필요한 것을 찾기 위해 문서를 더 많이 검색해야 합니다.
3. **렌더링된 컴포넌트 Mocking**:
	- 컴포넌트 렌더링의 모범 사례는 프런트엔드 라이브러리에 권장되는 테스트 프레임워크를 사용하는 것입니다. 기본으로 설치되어 있는 도구를 사용하면 구성 오류를 처리할 필요가 없습니다. 가능하면 얕은 렌더링과 스냅샷을 사용하여 테스트 시간을 절약하고 렌더링된 컴포넌트의 핵심 기능에 집중하세요.
    - Jest는 주로 React와 함께 사용되며, Jasmine은 Angular와 함께 사용됩니다. 하지만 세 프레임워크 모두 모든 프론트엔드 라이브러리에서 사용할 수 있습니다​
    - React 에서 Mocha, Enzyme, Chai 를 사용 하는 것도 가능하다.


API 비동기 호출 시 사용할 수 있는 라이브러리
- Jest, Jasmine 은 -> Supertest https://arc.net/l/quote/nhhftalo
- Mocha 는 -> chai-http


이 글을 보고 각 테스트 러너를 어떤식으로 비교하고 있는지 참고해볼수 있고 몇가지 비교 키워드를 알게 됐다.
지금 부터는 직접 비교해가며, 나의 의견을 정리하는 것이 필요할 것 같다

이를 토대로 정리해볼 주제들을 나열해보자
- 어떤면에서 Mocha 와 Jasmine 이 벡엔드 테스팅에 유리할까
- Jest 사용과 Mocha + Enzyme + Chai 사용 비교해보면 어떨까
- Jest의 스냅샷 기능은 어떤 강점이 있을까
- Jest 를 벡엔드에서 사용하는건 어떤 불편점이 있을까
- Node 에서 Mocha 를 사용하는 건 어떤 장점이 있을까


https://www.lambdatest.com/blog/jest-vs-mocha-vs-jasmine/
https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco




  
Jasmine이 Angular와 자주 사용되고, React가 Jest와 자주 사용되는 이유에는 여러 가지가 있습니다:

1. **Jasmine과 Angular**:
    
    - **내장된 지원**: Angular CLI는 기본적으로 Jasmine을 테스트 프레임워크로 사용합니다. 이는 Angular 개발자들이 별도의 설정 없이 바로 Jasmine을 사용할 수 있게 해줍니다.
    - **행동 주도 개발(BDD) 스타일**: Jasmine은 BDD 스타일의 테스팅을 제공하는데, 이는 Angular 개발의 설계 철학과 잘 어울립니다. BDD는 애플리케이션의 기능을 자연어처럼 기술하고 테스트하는 방식을 말합니다.
    - **역사적 배경**: AngularJS(Angular의 초기 버전) 시절부터 Jasmine이 널리 사용되었습니다. 이로 인해 Angular 커뮤니티에서 Jasmine에 대한 광범위한 지식과 경험이 축적되었습니다.
2. **Jest와 React**:
    
    - **Facebook에 의한 개발**: Jest는 React를 개발한 Facebook에 의해 만들어졌습니다. 이는 React 개발자들에게 Jest를 자연스러운 선택으로 만듭니다.
    - **최적화된 통합**: Jest는 React의 특성과 잘 맞도록 최적화되어 있으며, React의 UI 컴포넌트 테스팅에 유용한 기능들을 제공합니다.
    - **스냅샷 테스팅**: Jest의 스냅샷 테스팅 기능은 React 컴포넌트의 렌더링 결과를 쉽게 테스트할 수 있게 해줍니다. 이는 UI의 변경 사항을 추적하고 확인하는 데 매우 효과적입니다.
    - **React 커뮤니티의 지원**: React 커뮤니티는 Jest를 많이 사용하고, 이에 대한 풍부한 리소스와 지원을 제공합니다.

이러한 이유로, Angular 개발자들은 자연스럽게 Jasmine을, React 개발자들은 Jest를 선호하는 경향이 있습니다. 물론, 이는 필수적인 것은 아니며 다른 테스트 프레임워크를 사용하는 것도 가능하지만, 각 프레임워크와 라이브러리 간의 긴밀한 통합과 지원은 이들의 조합을 매우 인기있게 만듭니다.



## Jest 의 스냅샷
----
일단 Jest 에서 제공하는 대표적인 스냅샷 테스팅

- 인라인 스냅샷 생성 + 갱신
- 파일 스냅샷 생성 + 갱신
	- `__snapshots__` 에 스냅샷 파일 생성

그리고 스냅샷 테스팅의 단점을 경고하는 글
https://velog.io/@k7120792/%EB%B2%88%EC%97%AD-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%8A%A4%EB%83%85%EC%83%B7-%ED%85%8C%EC%8A%A4%ED%8C%85%EC%9D%98-%EB%8B%A8%EC%A0%90%EB%93%A4-o8jypesqnr

요약해보면 스냅샷 테스팅은

- 테스트 코드 변경없이 너무 쉽게 테스트를 수행
- 실패가 떠도 뭐가 어떻게 잘못됐는지 알길이 없다
- 빈약한 테스팅 행위
	- https://blog.imqa.io/testing-framework-jest/

등등을 말하고 있다.


