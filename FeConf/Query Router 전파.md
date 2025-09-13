RSC
TanStack 과 RSC
RSC 쿼리 방법론

RSC
서버에서만 알아야 하는 것들이 있다
- 서버만 알고있는 정보를 활용
- 더빠른 응답, 암호화처리, 파일 처리


TanStack 과 RSC
- prefetchQuery + HydrationProvider
- RSC 를 useQuery 와 가까이 둔다

위 방법 단점
- prefetch 로 복잡성 증가
- 직렬화 설계 잘못 했을 시 문제가 될 수 있음

사례
- 토스 김도한님의 제안 아래에서 위로 올려주는 resource 구조 제안
- Isograph 강타입을 통해서 아래에서 위로 올려주는 resource 구조 제안

새로운 방향
옆에 두는 구조로 풀어보자
Componsnet
- lindex.tsx
- loader.tsx
- client.tsx


새로운 프레임워크 제안
DataReady

쿼리 정의
- 캐시 가능한
- 비동기
- 읽기 연산
- 같은 키 - 같은 쿼리


requiredResources
-> dataread().use() 로 대체

