[관련 영상](https://www.youtube.com/watch?v=WZza2oISgJ8)
계속적으로 DOM 을 확인하면서, 테스팅을 해가면 도움이 된다. 이때 아래 함수를 활용하는데
screen.debug() 인자로 무엇을 넣냐에 따라서 보여지는 DOM의 영역이 정해진다.

내가 확인하고자 하는 container 를 넣는것이 더 확인하기 좋다.

```typescript
const button = screen.getByRole('button')
screen.debug(button)
```


검증하는 expect() 에 있는 함수지만, 특정 컴포넌트에 텍스트가 있는지 여부를 확인하는데에도 유용하다.
expect().toHaveAccessibleName


```typescript
const button = screen.getByRole('button')
expect(button).toHaveAccessibleName('OFF');
```