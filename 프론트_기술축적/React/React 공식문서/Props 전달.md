부모 컴포넌트는 props를 줌으로써 몇몇의 정보를 자식 컴포넌트에게 전달할 수 있다.

React 는 단방향 상태 전달을 기본으로 하고 있고, 부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달하는 가장 기본 방식이다.

# Props 란?
props는 JSX 태그에 전달하는 정보입니다.


# 전달 방법
객체 전달 -> 중괄호 안에 객체
리터럴 전달

```jsx
export default function Profile() {  
	return (  
		<Avatar  
			person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}  
			size={100}  
		/>  
	);
}
```


# 전달받은 Props 읽기
`({`와 `})` 안에 그들의 이름인 `person, size` 등을 쉼표로 구분

```jsx
function Avatar({ person, size }) {  
// person과 size는 이곳에서 사용가능합니다.  
}
```


# 이게 뭐가 좋나요?
props를 사용하면 부모 컴포넌트와 자식 컴포넌트를 독립적으로 생각할 수 있다.

- 부모 컴포넌트는 자식 컴포넌트가 props 를 어떻게 사용하는지 관심 없습니다. 필요해 하는 정보를 넘겨주기만 하면 됩니다.
- 자식은 전달받은 정보를 마음껏 변경해도 됩니다. 전달받은 props 를 어떻게 가공할 것이냐는 자식의 몫입니다.


## 논의거리
props 를 컴포넌트에서 타입을 지정하거나, 선언을 할 시 어떻게 하시나요?
- 분해할당을 하나요
- 한꺼번에 넘기고 참조로 값을 꺼내나요


## JSX spread 문법으로 props 전달하기
props 전달이 계속되는 계층으로 내려가다보면, 반복적인 전달이 이뤄지고 반복되는 코드가 발생합니다.

이런 반복되는 props 에 대해서는 spread 문법을 활용합니다.

```jsx
function Profile(props) {  
	return (
		<div className="card">  
			<Avatar {...props} />  
		</div>  
	);  
}
```

❌주의!!
**spread 문법은 제한적으로 사용하세요**. 다른 모든 컴포넌트에 이 구문을 사용한다면 문제가 있는 것입니다. 

이는 종종 컴포넌트들을 분할하여 자식을 JSX로 전달해야 함을 나타냅니다. 아래와 같이

```jsx
<Card>  
	<Avatar />  
</Card>

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```
JSX 태그 내에 콘텐츠를 중첩하면, 부모 컴포넌트는 해당 콘텐츠를 `children`이라는 prop으로 받습니다.

`children` prop을 가지고 있는 컴포넌트는 부모 컴포넌트가 임의의 JSX로 “채울” 수 있는 “구멍”이 있는 것으로 공식문서에 표현하고 있음

그만큼 이 패턴은 유연하다는 것을 강조하는 것 같다!


## 시간에 따라 Props 가 변한다?
변한다는건? 예: 사용자의 상호작용이나 새로운 데이터에 대한 응답)

props는 컴퓨터 과학에서 “변경할 수 없다”라는 의미의 [불변성](https://en.wikipedia.org/wiki/Immutable_object)을 가지고 있습니다. 부모 컴포넌트에 _다른 props_를 전달하도록 요청해야한다. 그러고 리랜더링이 일어난다.

하지만!! 이렇게 변해야하는 상태라면 State 로 정의하는 것이 맞다.


## 꺼낼만한 주제
props 전달은 몇개 까지가 적절한가
props 전달은 몇 뎁스 까지가 적절한가


## [[TypeScript]]
type 키워드 쓸래
interfact 키워드 쓰래


## 승연님이 꺼낸 주제
props 정의 할때 {} : {} 이어서 정의하는 것 어떠냐


---
#### 참고 자료
https://ko-react-exy5xcwjj-fbopensource.vercel.app/learn/passing-props-to-a-component