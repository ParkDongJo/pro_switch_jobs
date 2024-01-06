


## Test Runner
----

### 일반적인 비교
- Mocha, Jest, Jasmine은 모두 인기 있는 프레임워크로, 활발한 커뮤니티와 오랜 개발 기간을 가지고 있습니다.
- Mocha와 Jasmine은 Node 애플리케이션용으로 처음 구축되었기 때문에 백엔드 테스팅에 더 강점을 보입니다. 이들은 Jest보다 더 많은 백엔드 도구와 문서를 제공합니다.
- 프론트엔드 테스팅 프레임워크 선택은 주로 사용하는 프론트엔드 프레임워크에 영향을 받습니다. 예를 들어, Jasmine은 Angular와 함께 자주 사용되며, Jest는 React와 함께 사용하기 위해 Facebook에 의해 개발되었습니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.

### 비교 포인트
1. **함수 Mocking**:
    - 함수 호출은 애플리케이션에서 가장 일반적으로 테스트되는 부분입니다.
    - 테스트는 함수 호출의 기대 결과에 초점을 맞추고, 애플리케이션 상태에 변경을 가하지 않으며, 의도하지 않은 부작용이 테스트에 영향을 미치지 않도록 Mock 함수를 사용해야 합니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.
2. **데이터 Mocking**:
    - 백엔드에서 테스팅은 프론트엔드와 마찬가지로 까다롭습니다. 특히 데이터를 다룰 때 실제 데이터베이스에 데이터를 삽입하는 위험을 피하기 위해 테스트 데이터베이스에 Mock 데이터를 설정하는 것이 좋습니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.
3. **비동기 호출 Mocking**:
    - 비동기 코드는 문제를 일으킬 수 있으므로 테스트가 중요합니다.
    - 모든 테스트 프레임워크는 비동기 코드를 작성하는 데 여러 옵션을 제공합니다. 예를 들어, 콜백을 사용하거나, 더 읽기 쉽고 테스트가 중단되는 지점을 빨리 찾을 수 있는 async/await 패턴을 사용할 수 있습니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.
4. **렌더링된 컴포넌트 Mocking**:
    - 렌더링된 컴포넌트가 예상대로 사용 가능한지 확인하는 것도 중요한 테스트입니다.
    - Jest는 주로 React와 함께 사용되며, Jasmine은 Angular와 함께 사용됩니다. 하지만 세 프레임워크 모두 모든 프론트엔드 라이브러리에서 사용할 수 있습니다​


API 비동기 호출 시 사용할 수 있는 라이브러리
Jest, Jasmine 은 -> Supertest https://arc.net/l/quote/nhhftalo
Mocha 는 -> chai-http



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