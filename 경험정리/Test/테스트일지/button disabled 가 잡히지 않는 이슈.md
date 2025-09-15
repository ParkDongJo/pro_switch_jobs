## 현상
button 의 disabled 테스트를 아래와 같이 했을 때, disabled 가 "" 빈값이라도 이를 추적하지 못했다.

```typescript
expect(screen.getByText('인증')).toBeDisabled();
```


## 원인
button 는 sirius-ui 라는 공통컴포넌트로 되어 있고, button 텍스트를 넘길때 children 으로 넘기고 있다.
children 으로 넘길 때, 공통컴포넌트 내부에서 아래와 같이 감싸고 있다

```typescript
const Button = (children) => {
	return (
		<span>{children}</span>
	)
}
```

이때 getByText 로 엘레멘트를 타겟팅 했기 때문에, span이 타겟팅으로 잡힌다. button 의 disabled 를 테스트 하는 경우이기 때문에 정확히 button 을 타겟팅해야한다.


```typescript
expect(screen.getByRole('button', { name: '인증' })).toBeDisabled();
```


## 방향
습관적으로 Text 를 통해서 타겟팅하게 되는 경우가 있지만, 실제로는 웹접근성을 우선적으로 엘레멘트를 타겟팅해야한다.
그러기 위해서는 컴포넌트는 웹접근성을 지키도록 노력해야한다.