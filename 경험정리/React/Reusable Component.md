


# Polymorphic 한 컴포넌트
https://kciter.so/posts/polymorphic-react-component/

Phlymorphism 은 한글로 다형성 이라고 불린다.

- 다양한 Semantic 을 표현할 수 있는 UI 컴포넌트
- 다양한 속성/ 스타일 을 가질 수 있는 UI 컴포넌트


## 문제 인식

아래 코드는 <Button /> 이라는 재사용성이 높은 컴포넌트를 만들어 놓고,

a 링크로도 확장한 <LinkButton /> 을 만들어서 사용하고 있다.

```jsx
/**
 * Button.jsx
 */
export const Button = ({ ...props }) => {
  return (
    <button 
      style={{ backgroundColor: 'black', color: 'white' }} 
      {...props} 
    />
  );
}

/**
 * LinkButton.jsx
 */
import { Button } from './Button';

export const LinkButton = ({ href, ...props }) => {
  return (
    <a href={href}>
      <Button {...props} />
    </a>
  );
}

/**
 * App.jsx
 */
import { LinkButton } from './LinkButton';

const App = () => {
  return (
    <div>
      <LinkButton href="https://kciter.so">Click Me!</LinkButton>
    </div>
  );
}
```

지금의 내 수준이 만들어 낼 만한 가장 기본적인 형태의 컴포넌트이다.

하지만 이 컴포넌트의 한계는 아래와 같다.

- a 태그가 더이상 확장되지 않는다.
- LinkButton 는 의존관계가 심하다.
	- 만약 a 태그가 아닌 nextjs 의 <Link /> 태그를 사용해야 한다면, 새롭게 만들어줘야한다.


## JavaScript 로 구현

저자는 이해를 돕기 위해 먼저 JavaScript 로 구현해내고 있다. 일단 JavaScript는 type이 유연하여 구현을 하기 어렵지 않기 때문이다.


```jsx
/**
 * Button.jsx
 */

export const Button = ({ as, ...props }) => {
  const Element = as || 'button';
  return (
    <Element
      style={{ backgroundColor: 'black', color: 'white' }} 
      {...props} 
    />
  );
}

/**
 * App.jsx
 */
import { Button } from './Button';

const App = () => {
  return (
    <div>
      // 마치 앵커 태그처럼 사용할 수 있다.
      <Button as="a" href="https://kciter.so">Click Me!</Button>
    </div>
  );
}
```


## TypeScript 로 구현

여기까지는 어렵지 않았는데, TypeScript 로 넘어오면 복잡해지기 시작한다.


### 요소와 속성 표현

우선 타입 정의가 필요하다.

```tsx
/**
 * View.tsx
 */
interface ViewProps<T extends React.ElementType> {
  as?: T;
}
export const Button = <T extends React.ElementType = "div">({
  as,
  ...props
}: ViewProps<T>) => {
  const Element = as || "div";
  return <Element {...props} />;
};

const App = () => {
  return (
    // 컴포넌트 부분에 에러가 발생한다.
    <Button as="a" href="https://kciter.so">
      Link
    </Button>
  );
}

export default App;
```

위와 같이 작성하면 App 쪽의 Button을 사용하는 부분에서 에러 메시지가 뜬다.
에러 메시지를 살펴보면 props 로 넘긴 값들이 타입에 맞지 않다는 내용이다.

이때, 제시하는 방법은 ComponentPropsWithoutRef 를 활용하는 방법이다.

> **ComponentPropsWithoutRef** - `ref`를 제외한 나머지 속성을 정의할 수 있게 해주는 타입

```tsx
type ViewProps<T extends React.ElementType> = {
  as?: T;
} & React.ComponentPropsWithoutRef<T>;

export const Button = <T extends React.ElementType = "div">({
  as,
  ...props
}: ViewProps<T>) => {
  const Element = as || "div";
  return <Element {...props} />;
};
```


### ref 받아오기
이제 ref 받아 오도록 만든다

forwardRef를 활용하고, 두번째 인자로 들어오는 ref 받아서 <Element />에 넣어준다.

```tsx
type ViewProps<T extends React.ElementType> = {
  as?: T;
} & React.ComponentPropsWithoutRef<T>;

export const Button = forwardRef(
  <T extends React.ElementType = "div">(
    { as, ...props }: ViewProps<T>,
    ref: React.ComponentPropsWithRef<T>["ref"] // ref만 받아오도록
  ) => {
    const Element = as || "div";
    return <Element ref={ref} {...props} />;
  }
);
```

좀 더 디테일한 부분을 다루는데, 여기까지 구현체는 <App /> 사용부에서 type safe 가 지켜질 수 없다.

아래와 같은 경우를 들고있는데,

```tsx
const App = () => {
  const ref = useRef<HTMLDivElement>();
  return (
    // 아래와 같이 ref 는 div 의 ref 이고
    // as는 'a' 로 정의해줬지만
    // 에러를 잡지 못한다.
    <Button as="a" ref={ref} href="https://kciter.so">
      Link
    </Button>
  );
}

```

이런 현상이 일어나는 이유는 위에 <Button /> 안에 감싸고 있는 forwardRef() 에 넘겨지는 함수컴포넌트의 타입정의가 모호하기 때문이다.

forwardRef 반환 타입을 살펴보고 <Button /> 코드를 좀 더 개선해보면

```tsx
// 입력 타입
type ViewProps<T extends React.ElementType> = {
  as?: T;
} & React.ComponentPropsWithoutRef<T>;

// 반환 타입
type ViewComponent = <C extends React.ElementType = "div">(
  props: ViewProps<C> & {
    ref?: React.ComponentPropsWithRef<C>["ref"];
  }
) => React.ReactElement | null;

export const Button: ViewComponent = forwardRef(
  <T extends React.ElementType = "div">(
    { as, ...props }: ViewProps<T>,
    ref: React.ComponentPropsWithRef<T>["ref"]
  ) => {
    const Element = as || "div";
    return <Element ref={ref} {...props} />;
  }
);
```

위와 같이 하면

```tsx

// 에러 발생
const App = () => {
  const ref = useRef<HTMLDivElement>();
  return (
    // 에러가 발생하고
    <Button as="a" ref={ref} href="https://kciter.so">
      Link
    </Button>
  );
}

// 정상 타입추론
const App = () => {
  // ref 타입 HTMLAnchorElement
  const ref = useRef<HTMLAnchorElement>();
  return (
    // 에러가 발생하고
    <Button as="a" ref={ref} href="https://kciter.so">
      Link
    </Button>
  );
}

```

여기서 더 나아가 타입을 추상화 해볼 수 있겠다.


# Render delegation

https://kciter.so/posts/render-delegation-react-component/

이전에 소개된 Polymorphic한 React 컴포넌트 는 몇가지 단점을 가지고 있다.

- prop 이 어떤 컴포넌트인지 알기 힘들다
	- 렌더링 깨짐
	- 모든 케이스에 대한 대비
- 코드만 봐서는 렌더링 흐름을 이해하기 힘들다
- type 추론을 하기 어렵다

이 단점에 대한 대한으로 Render delegation 방법이 소개 되고 있다. Radix 라는 오픈소스 리엑트 컴포넌트 라이브러리를 통해 유명해진 방법이라고 한다. (시간나면 한번 까보는 것도 좋을것 같다.)

Polymorphic 한 컴포넌트
공통점 - 다형성 문제해결을 위한 방법 이라는 점
차이점 - **기존 컴포넌트와 합성이될 컴포넌트를 코드에서 분리한다는 점**

## Slot 과 Slottable
이 두가지 요소로 구성된다.

Slot - 자식 컴포넌트를 렌더링 하는 역할
Slottable - Slot 에 들어갈 것이 무엇인지 나타냄

**기존 컴포넌트와 합성이될 컴포넌트를 코드에서 분리한다는 점** 이 다르다


## Radix 로 엿보기

#### Slot 사용해보기
우선 이해를 돕기위해 JavaScript 로 해보자

```jsx
import { Slot } from "@radix-ui/react-slot";

const Button = ({ asChild, ...props }) => {
  const Element = asChild ? Slot : "button";
  return (
    <Element 
      {...props}
      style={{
        padding: "10px",
        border: "1px solid #000",
        borderRadius: "5px",
        backgroundColor: 'transparent',
        fontSize: 12
      }}
    />
  );
};

const App = () => {
  return (
    <div>
      <Button>
        This is button
      </Button>

      <Button asChild>
        <a href="https://kciter.so">This is link</a>
      </Button>
    </div>
  );
};
```

asChild 가 true -> Slot 컴포넌트는 props 로 받은 children JSX 요소를 렌더링 한다.
asChild 가 false -> Button 요소를 사용

#### Slottable 사용해보기

```jsx
import { Slot, Slottable } from "@radix-ui/react-slot";

const Icon = () => (
  <span>🔴</span>
)

const Button = ({ asChild, icon, children, ...props }) => {
  const Element = asChild ? Slot : "button";
  return (
    <Element 
      {...props}
      style={{
        padding: "10px",
        border: "1px solid #000",
        borderRadius: "5px",
        backgroundColor: 'transparent',
        fontSize: 12
      }}
    >
      {icon}
      <Slottable>{children}</Slottable>
    </Element>
  );
};

const App = () => {
  return (
    <div>
      <Button icon={<Icon />}>
        This is button
      </Button>

      <Button icon={<Icon />} asChild>
        <a href="https://kciter.so">This is link</a>
      </Button>
    </div>
  );
};
```

사실 사용법을 보면 매우 간단한 개념이다

Slottable 은 Slot 컴포넌트로서 렌더링 될 children 의 위치를 정해줄 수 있다.


## 직접 구현해보기

당연히 라이브러리를 사용하면 쉽겠지만, 공부를 하는 차원에서 직접 구현하고 그 원리를 이해하는 것이 중요하다
직접 구현한다면 아래와 같을 것이다.

```jsx
/**
 * Slot.jsx
 */

import React from "react";

export const Slot = ({ children, ...props }) => {
  if (React.isValidElement(children)) {
    return React.cloneElement(children, {
      ...props,
      ...children.props,
    });
  }

  // 올바르지 않은 컴포넌트라면 경고 로그를 출력하고 null을 반환
  console.warn("Slot component should have only one React element as a child");

  return null;
};
```


결국 React 의 cloneElement 를 사용해서 children 으로 받아온 컴포넌트를 뱉어준다. valid 하지 않다면 null 로 반환한다.


```jsx
/**
 * Slottable.jsx
 */

export const Slottable = ({ children }) => {
  return <>children</>;
}
```

Slottable 자체는 어려울 것이 없는데, Slottable 로 인해, Slot 의 내부 로직도 변경이 되어야 한다.

```jsx
import React from "react";
import { Slottable } from "./Slottable";

export const Slot = ({ children, ...props }) => {
  const childrenArray = React.Children.toArray(children);
  const slottable = childrenArray.find((child) => {
    return React.isValidElement(child) && child.type === Slottable
  });

  if (slottable) { // Slottable이 있다면
    const newElement = slottable.props.children;
    const newChildren = childrenArray.map((child) => {
      // Slottable이 아닌 것은 그대로 반환
      if (child !== slottable) return child;

      // Slottable이라면 해당 영역을 자식 컴포넌트의 children으로 교체
      if (React.isValidElement(newElement)) {
        return newElement.props.children;
      } else {
        console.warn(
          "Slot component should have only one React element as a child"
        );
      }

      return null;
    });

    // 새로운 요소를 렌더링
    return React.isValidElement(newElement)
      ? React.cloneElement(
          newElement, 
          { ...props, ...newElement.props }, 
          newChildren
        )
      : null
  }

  if (React.isValidElement(children)) {
    return React.cloneElement(children, {
      ...props,
      ...children.props,
    });
  }

  console.warn("Slot component should have only one React element as a child");

  return null;
};
```

어려울 건 없다. 기본적인 구조는

- slottable 이냐?
	- slottable 이면 slottable 안의 children 뽑아서 React.cloneElement 로 넘긴다.  
- 일반 children 이냐?
	- children 을  Reac.cloneElement 로 넘긴다

의 차이일 것이다.

타입스크립트로의 변경은 직접 적용을 해보면서, 한번더 정리해보자. 그것보다는 좀 더 나아가서 추가 구현을 따라가보자

## 고도화

- ref 받아오기
- prop 병합 문제

이 부분은 특수한 상황에 대한 대비이다. 정말 재사용성이 높은 컴포넌트를 만드는 길은 멀고도 험하다.

#### ref
타입추론을 그때마다 하기 위해서, ref 는 어디에 넣어야 할까?

```tsx
<Button asChild ref={???}>
  <a href="https://kciter.so" ref={???}>
    Click me!
  </a>
</Button>
```

글쓴이는 Radix 라이브러리 내부에서 해답을 찾고 있다.

```tsx
type PossibleRef<T> = React.Ref<T> | undefined;

// ref를 설정하는 함수
function setRef<T>(ref: PossibleRef<T>, value: T) {
  if (typeof ref === "function") {
    ref(value);
  } else if (ref !== null && ref !== undefined) {
    (ref as React.MutableRefObject<T>).current = value;
  }
}

// ref를 합성하는 함수
function composeRefs<T>(...refs: PossibleRef<T>[]) {
  return (node: T) => refs.forEach((ref) => setRef(ref, node));
}
```

라이브러리 안에 아래 두가지 함수가 있는듯 하다

setRef -> ref 를 설정해주는 함수이고
composeRefs -> 여러 ref 들을 node 에 setRef 해준다.

즉, 어떤 ref 가 들어와도 하나로 합성을 해주는걸로 보인다.