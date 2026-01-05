
[https://serhiikoziy.medium.com/feature-sliced-design-architecture-in-react-with-typescript-447dc5e6a411](https://serhiikoziy.medium.com/feature-sliced-design-architecture-in-react-with-typescript-447dc5e6a411)

[https://blog.bizspring.co.kr/%ED%85%8C%ED%81%AC/%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9D%B8-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EC%84%A4%EA%B3%84%EB%A5%BC-%EC%9C%84%ED%95%9C-feature-sliced-designfsd%EC%9D%98-%EC%9D%B4%ED%95%B4/](https://blog.bizspring.co.kr/%ED%85%8C%ED%81%AC/%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9D%B8-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EC%84%A4%EA%B3%84%EB%A5%BC-%EC%9C%84%ED%95%9C-feature-sliced-designfsd%EC%9D%98-%EC%9D%B4%ED%95%B4/)

FSD  
![Pasted image 20250319102319.png](app://9a6a7e547e2ae5fa039d82d1b4186da4dc0c/Users/a13656/42dot_dongjo/pro_switch_jobs/images/Pasted%20image%2020250319102319.png?1742347399826)

1. **App Layer**: Handles application-wide concerns like routing, theming, and state management providers.
2. **Processes**: Business workflows that combine multiple features, such as authentication flows.
3. **Pages**: Feature aggregates representing a single route or a complete screen.
4. **Features**: Encapsulates a specific, independent piece of functionality (e.g., a search bar or user profile).
5. **Entities**: Domain-level components or state representations (e.g., `User`, `Product`).
6. **Shared**: Reusable utilities, hooks, components, or constants used across the app.

pages -> pages

- suspense 사용으로 인해 pages 안의 컴포넌트가 빈약한것  
    model -> store  
    features / widgets -> features

구조의 핵심 features 가 너무 무겁다

- features 에 있는 것들을 다른 곳으로 이동시켜야한다

pages 의 활용도를 높일 필요가 있어보임

- suspense 사용으로 인해 pages 안의 컴포넌트가 빈약한것
- 예를들어 AboutMe 같은 경우
    - View 에 있는 녀석들은 pages 로 옮김
    - AboutMeDialog 같은 녀석도 해당 pages 에서만 사용된다면 features 에 있는게 아니라 pages 하위에 구조를 가져가는 것이 맞을거 같음
    - 이런 녀석들이 은근 있을거 같음 (나도 그걸 보고 참고햇으니)
        - 주로 외주회사의 작품인듯...
            - VehicleMaintenance
            - VehicleGroup
            - Setting

features 의 정의가 다시 필요함

- features 는 컴포넌트의 묶음 (사실 widget 같은 느낌) 을 한번에 제공한다는 느낌
- 아래와 같이 search 라는 개념 하위아래 search bar, filter, input, 그 결과목록 등등이 하나의 패키지로features로 제공되는 느낌
- 근데 문제는 이 features 의 덩치가 큰 녀석들이 있고
- features 가 화면, pages 를 기반으로 묶여있는 녀석들이 있음
    - 예를 들어 vehicle 차량과 관련된 것들이면 모두 vehicle 아래로 위치!
    - vehicle 하위에도 분리가 가능해보이는 것들이 존재  
        features 가 덩치가 점점 커질 수 밖에 없는 이유가
- 우리가 쓰는 구조가
- features > vehicle 이 있으면 여기서 부터 subroot 느낌으로 지정됨
    - api, components, store 등등
    - 그런데 이걸 좀 분리를 하려면 둘중 하나일 거 같다는 생각
        - features 에서 또 하나의 덩어리로 될거 같은 덩어리를 feature 단위로 승격 시키거나
        - 또는 features > ... > ... > battery 이렇게 하위 구조에 api, store, ui 등등을 배치시킨다던지  
            ![Pasted image 20250318224735.png](app://9a6a7e547e2ae5fa039d82d1b4186da4dc0c/Users/a13656/42dot_dongjo/pro_switch_jobs/images/Pasted%20image%2020250318224735.png?1742305655814)

그런데 지금은 entities 의 역할이 함께 혼재된 느낌