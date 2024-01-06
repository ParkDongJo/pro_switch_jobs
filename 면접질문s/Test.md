


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
3. **데이터 Mocking**:
    
    - 백엔드에서 테스팅은 프론트엔드와 마찬가지로 까다롭습니다. 특히 데이터를 다룰 때 실제 데이터베이스에 데이터를 삽입하는 위험을 피하기 위해 테스트 데이터베이스에 Mock 데이터를 설정하는 것이 좋습니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.
4. **비동기 호출 Mocking**:
    
    - 비동기 코드는 문제를 일으킬 수 있으므로 테스트가 중요합니다.
    - 모든 테스트 프레임워크는 비동기 코드를 작성하는 데 여러 옵션을 제공합니다. 예를 들어, 콜백을 사용하거나, 더 읽기 쉽고 테스트가 중단되는 지점을 빨리 찾을 수 있는 async/await 패턴을 사용할 수 있습니다​[](https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco)​.
5. **렌더링된 컴포넌트 Mocking**:
    
    - 렌더링된 컴포넌트가 예상대로 사용 가능한지 확인하는 것도 중요한 테스트입니다.
    - Jest는 주로 React와 함께 사용되며, Jasmine은 Angular와 함께 사용됩니다. 하지만 세 프레임워크 모두 모든 프론트엔드 라이브러리에서 사용할 수 있습니다​



https://www.lambdatest.com/blog/jest-vs-mocha-vs-jasmine/
https://dev.to/heroku/comparing-the-top-3-javascript-testing-frameworks-2cco