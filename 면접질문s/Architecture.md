


## 모놀리식 vs. MSA
----



https://medium.com/coupang-engineering/how-coupang-built-a-microservice-architecture-fd584fff7f2b

https://medium.com/@coupang-engineering-kr/integrating-platform-services-to-a-microservice-architecture-6f19f7816e15


## VAC 패턴 / Presentational-Container
-----
React 컴포넌트를 설계하는데, 가장 많이하는 고민중에 하나는 비즈니스 로직을 어디다가 위치하는게 좋을까 이다.

여기서 비즈니스 로직에 정의를 정확히 내려야 할것 같다. 내가 정의하는 비즈니스 로직은
- 해당 도메인과 관련있거나 (바구니, 쿠폰, 결제)
- 사업영역과 관련된 (부동산, 쇼핑몰)

에 해당하는 코드들이다.

이런 로직들은 lib, util, hooks 에 따로 빼기가 애매할 때가 많다. 결국 특정 컴포넌트에 몰아넣거나, 무분별하게 퍼져있을 때가 많다.

나는 이것들을 container > hooks > lib, util 등등의 순으로 옮길 것을 고민한다.

이런 고민들과 연관해서 React 에서도 여러 패턴들이 있다.
정확한 용어가 정해지지 않은 패턴들은 자체적으로 이름을 붙이겠다.

##### VAC 패턴 -------
랜더링과 스타일을 관리하는 컴포넌트를 따로 떼어놓는 패턴이다.

Container - 
- UI 인터페이스
- state 관리
- 비즈니스 로직
VAC
- CSS, markup
- JSX 렌더링

그리고 또하나의 특징은 Props 를 Object 로 추상화 하고, 그걸 VAC 로 넘겨준다.
```javascript
// VAC
const SpinBoxView = ({ value, onIncrease, onDecrease }) => (
  <div>
    <button onClick={onDecrease}>-</button>
    <span>{value}</span>
    <button onClick={onIncrease}>+</button>
  </div>
);
```

```javascript
// View Component
const SpinBox = () => {
  const [value, setValue] = useState(0);

  const props = {
    value,
    onDecrease: () => setValue(value - 1),
    onIncrease: () => setValue(value + 1),
  };

  // JSX를 VAC로 교체
  return <SpinBoxView {...props} />;
};
```

##### Presentation - Container 패턴 -------
비즈니스 로직을 상위 컴포넌트인 Container 에 위치시키는 패턴입다.

Container : 
- 비즈니스 로직
- state 관리
Presentation :
- UI 인터페이스
- state 관리
- CSS, markup
- JSX 렌더링

이름만 다르지 VAC 와 굉장이 비슷하다. 다만 Presentation 컴포넌트는 VAC 컴포넌트보다 좀 더 역할의 범위가 크다. VAC 는 오직 스타일과 구조에 대해서만 담당한다.

![[Pasted image 20231224123246.png]]

##### with custom hooks 패턴 -------
커스텀 hooks 를 활용해서 공통된 비즈니스 로직과 state 를 빼는데 사용하는 패턴이다.

다만 hooks 를 통해서 비즈니스 로직을 분리시키는 코드나 블로그 글도 봤고, 나도 그런적이 있지만 지금은 커스텀 hooks 에 대한 관점이 조금 달라졌다.

무조건 비즈니스 로직들을 hooks 로 빼버리면, 어떤 부분에서는 hooks의 재사용성이 떨어질 수 있다. 특정 코드에서만 돌아가는 hooks는 사실 좋은 방향은 아니라고 생각한다.


##### Data management with Provider 패턴 -------
Context API 를 활용하여 state 를 관리하고, 전달 시 Provider 로 하위 컴포넌트들을 감싸서 전달하는 방법이다.

나는 아래와 같은 경우에 활용했는데,

- 공통 state 를 사용하는 하위 컴포넌트들이 많고
- 이 컴포넌트들을 하나의 덩어리 관심사로 묶어서 여러 군데 사용할 때
- 전역이 아닌 특정 공통 state 범위를 지정하고자 할때

사용을 했었다.

구현은 아래와 같이 한다.
```javascript
export const ThemeContext = React.createContext(null);

export function ThemeProvider({ children }) {
  const [theme, setTheme] = React.useState("light");

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

```jsx
impor { useContext } from 'react';
import { ThemeProvider, ThemeContext } from "../context";


const HeaderSection = () => {
  <ThemeProvider>
    <TopNav />
  </ThemeProvider>;
};


const TopNav = () => {
  const { theme, setTheme } = useContext(ThemeContext);
    
  return (
    <div style={{ backgroundColor: theme === "light" ? "#fff" : "#000 " }}>
      ...
    </div>
  );
};

```

##### Hoc ------
고차 컴포넌트를 활용하는 패턴이다. 이 고차 컴포넌트는 컴포넌트를 인자로 받아서, 추가 state 나 기능을 주입된 완성품 컴포넌트를 반환한다.

React 공식 문서에서는 class 컴포넌트 기반으로 예제가 나와있지만, 개인적으로 Functional 컴포넌트 기반으로 만들어진 고차 컴포넌트도 동일한 원리라고 본다.

나는 개인적으로 아래와 같은 경우 사용한다.
- 공통로직이 상위 컴포넌트로 분리가 가능할 때
- 비슷한 구조를 가진 하위 컴포넌트들이 2개 이상 있을 때

```jsx
import React from 'react'

const higherOrderComponent = Component => {
  return class HOC extends React.Component {
    state = {
      name: 'John Doe'
    }
     
    render() {
      return <Component name={this.state.name {...this.props} />
    }
 }
  
  
const AvatarComponent = (props) => {
  return (
    <div class="flex items-center justify-between">
      <div class="rounded-full bg-red p-4">
          {props.name}
      </div>
      <div>
          <p>I am a {props.description}.</p>
      </div>
    </div>
  )
}
      
const SampleHOC = higherOrderComponent(AvatarComponent);

const App = () => {
  return (
    <div>
      <SampleHOC description="Frontend Engineer" />
    </div>
  )
}

export default App;
```


##### Compound Components (복합적인 컴포넌트) -------
여러 자식 컴포넌트들로 구성되어서 하나의 모듈 형식으로 제공되는 컴포넌트를 관리하기 위한 패턴이다.

나는 아래와 같은 상황에서 고려한다.
- 여러 자식 컴포넌트를 재구성해서 사용하는 구조일때
- 하나의 큰 관심사로 묶을 수 있는 하위 관심사들 일때

코드는 아래와 같이 해볼 수 있다.
```jsx
import React, { createContext, useState } from 'react';

const ToggleContext = createContext();

function Toggle({ children }) {
  const [on, setOn] = useState(false);
  const toggle = () => setOn(!on);

  return (
    <ToggleContext.Provider value={{ on, toggle }}>
      {children}
    </ToggleContext.Provider>
  );
}

Toggle.On = function ToggleOn({ children }) {
  const { on } = useContext(ToggleContext);
  return on ? children : null;
}

Toggle.Off = function ToggleOff({ children }) {
  const { on } = useContext(ToggleContext);
  return on ? null : children;
}

Toggle.Button = function ToggleButton(props) {
  const { on, toggle } = useContext(ToggleContext);
  return <button onClick={toggle} {...props} />;
}

function App() {
  return (
    <Toggle>
      <Toggle.On>The button is on</Toggle.On>
      <Toggle.Off>The button is off</Toggle.Off>
      <Toggle.Button>Toggle</Toggle.Button>
    </Toggle>
  );
}
```

##### forwardRef 패턴 -------
React 에서 제공하는 farwardRef 라는 고차 컴포넌트를 사용하는 패턴이다. 이 패턴을 사용하면 부모 컴포넌트가 하위 컴포넌트에서 open 해놓은 기능과 데이터를 참조해서 제어할 수 있다.

이 패턴은 무분별하게 이용하는 것은 좋지 않다. 일단 React 의 단방향 흐름에 위반이 되는 패턴이다. 그리고 코드가 복잡해지고 데이터 흐름을 파악하기 힘들어진다.

코드는 아래와 같다

```jsx
import React from "react";
 
const CustomInput = React.forwardRef((props, ref) => (
  <input type="text" {...props} ref={ref} />
));
 
const ParentComponent = () => {
  const inputRef = useRef(null);
 
  useEffect(() => {
    inputRef.current.focus();
  }, []);
 
  return <CustomInput ref={inputRef} />;
};
```

https://refine.dev/blog/react-design-patterns/#manage-custom-components-with-forwardrefs
https://itchallenger.tistory.com/593
