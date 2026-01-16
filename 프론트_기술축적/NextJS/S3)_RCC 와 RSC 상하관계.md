# 현상

RCC 에서 RSC 를 직접 호출하면 오류가 발생
해결 > RCC 의 children 으로 RSC 를 넘겨준다.

```jsx
export function ServerPage() {
  return (
    <ClientComponent>
      <ServerComponent />
    </ClientComponent>
  );
}
```

# 왜 오류가 발생해?

ServerComponent 를 만드는 과정은 아래와 같다.

서버에서는

- Page 로드 또는 리로드가 되면, 서버에 요청을 한다.
- 서버에서는 RSC 를 렌더링 하고, RSC Payload 라는 정보를 함께 만든다.
- NextJS는 서버에서 Payload 와 함께 Client JavaScript Instruction 참고해서 HTML 을 만든다.

클라이언트 (브라우저) 에서는

- 전달 받은 HTML 을 pre-rendering 을 한다.
- RCC와 RSC 트리를 구성해서 재조정을 할 때, RSC Payload 를 활용한다.
  - 이 때 RSC 와 RCC 의 관계를 tree 로 만든다.
  - 당장은 RCC는 placeholder (비어있는 노드) 로 남아있다.
  - RSC Payload 를 참고해서 placeholder 를 채운다.
- 가상 DOM 트리가 완성되면, 실제 DOM 과 비교하여 반영을 한다.

이 때 RCC 내부에 RSC 가 위치하게 되면 RSC 노드를 채워주지 못한다. RCC는 placeholder (비어있는 노드) 로 남아 있을 거기 때문이다.

# 그럼 왜? children 으로 넘기는건 괜찮아?

서버에서도 JSX 를 babel 이 트렌스파일링 하는 과정에서 아래와 같은 코드는

```jsx
export function ServerPage() {
  return (
    <ClientComponent>
      <ServerComponent />
    </ClientComponent>
  );
}
```

이렇게 트렌스 파일링 된다

```jsx
React.createElement(ClientComponent, {}, React.createElement(ServerComponent));
```

살펴보면, 부모 RSC 에서 RCC 내부로 전달되는 children RSC 까지 트렌스파일링 하여 결과를 만든다.

하지만 만약 ClientComponent 내부에 ServerComponent이 선언되어 있다면 ServerComponent 을 트렌스 파일링 하지 못한다.

# 왜 이렇게 까지 해야하는거야

기존의 설계 방식보다 불편하다! 그리고 설계 자체가 복잡해 진다. 하지만 React 진영은 React 의 성능향상을 서버쪽에서 풀었고, 이때 서버와 클라이언트에 대한 분리를 명확히 하고자 한다. 이런 방향은 아래와 같은 이점을 만들 수 있다.

- 서버에서는 RSC 를 책임지고, 클라이언트에서는 RCC를 책임진다. 역할 분리가 명확하다
- 서버에서 클라이언트로 단방향의 흐름으로 된다. 흐름이 깔끔하다!
- 덕분에 서버에서 1차 미완성본 html 을 내려주고, 이어서 클라이언트를 위한 bundle과 RSC Payload 가 내려간다. 이때 bundle 의 크기는 더 작은 상태여서, 빠르게 다운로드도 되고 브라우저 렌더링과 하이드레이션도 시간이 단축될 것이다.
