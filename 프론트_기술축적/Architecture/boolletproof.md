https://github.com/alan2207/bulletproof-react/blob/master/docs/project-structure.md




Bulletproof React랑 비슷한 레벨에서 이야기되는 아키텍처·폴더링 철학은 몇 가지가 있어. 각자 강조점이 달라서 “대체”라기보다는 “같은 문제를 다른 각도로 푼 것들”에 가깝다고 보면 좋아.[7][9]

## 많이 비교되는 것들

- Feature-Sliced Design (FSD)  
  - 러시아/유럽 쪽 커뮤니티에서 많이 쓰는 “feature-first + 계층” 아키텍처 패턴.[6]
  - layers (app / processes / pages / features / entities / shared)로 나누고, 의존 방향을 강하게 제한해서 점진적 확장을 쉽게 하는 게 특징이라 Bulletproof의 feature 기반 구조와 철학이 비슷함.[9]

- Atomic Design 기반 폴더링  
  - atoms / molecules / organisms / templates / pages로 UI 컴포넌트를 계층화하는 방식.[6]
  - Bulletproof처럼 “도메인/기능 단위”보다는 “UI 추상화 단계”에 초점을 두기 때문에, UI 라이브러리·디자인 시스템 쪽에 더 잘 맞음.[6]

- Helix Principle / Layered Frontend (app / features / foundation)  
  - Reddit에서도 Bulletproof에 대해 이야기할 때 자주 같이 언급되는 패턴으로, app, features, foundation 세 레이어로 나눠 의존성이 한 방향으로만 흐르도록 강제하는 철학.[5]
  - Bulletproof의 src/features, shared, ui 같은 분리와 잘 맞아서, 사실상 “이념적 상위 개념”처럼 섞어서 쓰기도 좋음.[9]

- Next.js 중심 아키텍처 템플릿들  
  - 최근 Bulletproof React도 Next.js App Router/Pages Router 버전을 포함하도록 업데이트되면서, Next.js 공식 문서에서 추천하는 “route segment 기반 + feature 폴더링”과 비교되는 경우가 많음.[13][7]
  - Next.js는 파일 시스템 라우팅·서버 컴포넌트·데이터 패칭 규칙까지 포함한 프레임워크 차원의 아키텍처를 제공해서, Bulletproof를 “프레임워크-불문 베스트 프랙티스 세트”로, Next 템플릿들을 “특정 프레임워크에 최적화된 아키텍처”로 보는 구도가 많음.[11]

## Bulletproof React랑의 관점 차이

- Bulletproof React  
  - “React 앱을 확장 가능한 구조로 만들기 위한 모범 사례 모음집”이라는 포지션.[7][11]
  - 도메인/기능 단위(features), 공유 모듈(shared), 테스트/도구 설정까지 포함한 **실전 템플릿**에 가깝고, 특정 라이브러리 선택(tanstack-query, react-hook-form, msw 등)까지 제안함.[11][9]

- 다른 패턴들  
  - FSD/Helix/Atomic 같은 것들은 “폴더 구조 샘플 레포”라기보다 **아키텍처 원칙/철학** 쪽에 가까움.[5][6]
  - 그래서 Bulletproof를 기반으로 가져가고, 그 위에 FSD나 Helix의 의존성 규칙, Atomic의 UI 계층화를 섞어서 쓰는 식으로 조합하는 경우가 많아.[5][9]

## 실무에서 선택할 때 팁

- “레포 예시 + 베스트 프랙티스 세트”가 필요하면  
  - Bulletproof React 템플릿 그대로 가져가서 팀 컨벤션으로 삼기 좋음.[7][11]

- “조직/제품 레벨에서 일관된 규칙”이 더 중요하면  
  - FSD나 Helix 같은 상위 아키텍처 원칙으로 의존 방향·폴더 규칙을 정하고, 세부 구현은 자유롭게 두는 방식이 더 나음.[5][6]

- Next.js를 메인으로 쓰는 경우  
  - Next 공식 추천 구조 + Bulletproof의 feature/shared 아이디어를 섞고, 필요하면 FSD식 계층을 덧씌우는 조합이 현실적으로 많이 쓰임.[13][11]

원하면 Bulletproof React 기준으로, FSD/Helix 섞어서 monorepo 구조까지 포함한 예시 트리 한 번 그려줄까?

출처
[1] React Application Architecture for Production : r/reactjs - Reddit https://www.reddit.com/r/reactjs/comments/1093ztw/react_application_architecture_for_production/
[2] 최고의 리액트 아키텍처 : r/react - Reddit https://www.reddit.com/r/react/comments/1fvci92/best_react_architecture/
[3] 갓생코딩일지 https://coding-god-life.tistory.com
[4] 왜 개발자들은 React를 계속 망치는 걸까? - Reddit https://www.reddit.com/r/react/comments/1ejigod/why_do_devs_keep_ruining_react/
[5] Bulletproof React just got updated! 🚀 - A simple, scalable, and powerful architecture for building production ready React applications. https://www.reddit.com/r/reactjs/comments/1crparb/bulletproof_react_just_got_updated_a_simple/
[6] 리액트 아키텍처 패턴 (React Architecture Patterns) https://velog.io/@sitaruta/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-React-Architecture-Patterns
[7] alan2207/bulletproof-react - GitHub https://github.com/alan2207/bulletproof-react
[8] BulletProof React https://github.com/alan2207/bulletproof-react/tree/master
[9] "bulletproof-react" is a hidden treasure of React best practices! https://dev.to/meijin/bulletproof-react-is-a-hidden-treasure-of-react-best-practices-3m19
[10] 2025 년 10 월 넷째 주, 위협 동향 보고서 (Threat Intelligence ... https://www.secui.com/download?unit=trends&id=2EC4092ABDDCFB7E8851365D68CA4271&no=0
[11] 2024-06 - naver/fe-news 뉴스레터 뷰어 (비공식) https://fe-news.atj.sh/issues/2024-06
[12] 가상 네트워크 관리를 위한 기계학습 기반 이상 탐지 시스템 설계 http://dpnm.postech.ac.kr/papers/KNOM/21/%EA%B0%80%EC%83%81%20%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%20%EA%B4%80%EB%A6%AC%EB%A5%BC%20%EC%9C%84%ED%95%9C%20%EA%B8%B0%EA%B3%84%ED%95%99%EC%8A%B5%20%EA%B8%B0%EB%B0%98%20%EC%9D%B4%EC%83%81%20%ED%83%90%EC%A7%80%20%EC%8B%9C%EC%8A%A4%ED%85%9C%20%EC%84%A4%EA%B3%84.pdf
[13] Bulletproof React가 Next.js용으로 업데이트됐어! : r/reactjs - Reddit https://www.reddit.com/r/reactjs/comments/1f7x8uw/bulletproof_react_has_been_updated_for_nextjs/
[14] React 파이버 아키텍처 분석 - NAVER D2 https://d2.naver.com/helloworld/2690975
