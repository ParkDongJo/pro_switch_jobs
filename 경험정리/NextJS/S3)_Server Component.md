https://jk6722.tistory.com/entry/RSC-vs-SSR


# 공식 문서 참고
https://nextjs.org/docs/app/getting-started/server-and-client-components

## 컴포넌트 정의
컴포넌트는 데이터를 인자로 받아서, JSX 를 return 하는 함수 또는 render 메서드를 제공하는 클래스

## 클라이언트 컴포넌트 렌더링
Babel 을 통해서 JSX -> ReactElement 로 변환
ReactElement 는 DOM 에 표현하기 위해 필요한 정보를 담고있는 객체
- Fiber (Node)
- Virtual DOM 의 Node

비교 알고리즘을 통해 Virtual DOM 을 Real DOM 에 반영 하는 과정


## 서버 컴포넌트 렌더링
Server 에서

NextJS 는 React API를 활용해서, Rendering Work 은 여러 chunk 로 분할된다.

아래 기준으로 분할된다
- Router Segment 로 Split 된다
- Suspense Boundary 

Sever 에서는
React 는 Server Component 를 Render -> RSC Payload 로 만든다
- Server Component render 결과물
- Client Component 의 render 위치 (빈 노드로 표시)
	- JavaScript 파일 위치
- RSC 가 -> RCC 에게 전달하는 props

[[NextJS]] 는 아래를 활용해서 HTML 을 render 한다.
- RSC payload 를 활용
- RCC JavaScript Instruction 을 활용


Client 는
1. 서버에서 만든 HTML 을 즉시 보여준다.
2. RCC와 RSC 트리를 구성해서 재조정을 할 때, RSC Payload 를 활용한다. 
	1.  RSC Payload 에 빈 노드로 정의된 위치를 RCC 로 채운다.
3. Hydrate 가 된다 
	1. RSC Payload 를 참고하여 빈 노드를 채운 RCC 의 Interaction(상호작용) 이 가능하도록 JavaScript Instruction 이 가능하도록 한다.