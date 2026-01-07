# 첫번째 레슨
위와 같은 마찬가지 이유로 getByRole('textbox', { name: 'aa' }) 으로 intput 요소를 찾지 못할 수가 있다.

하지만, input 에 name 이 지정되어 있는 상태라고 해보자!

이때 외부 라이브러리의 ui 라서 당장 웹접근성을 높이지 못한다면, 아래와 같은 방법으로 input 을 찾을 수 있다.

```jsx
const { container } = render();

const input = container.querySelector('input[name="reportStartDate"]'),
```

위와 같이 찾을 수 있다.

```jsx
expect(input).toHaveValue(startDate.format('YYYY.MM.DD'))
```


# 알고 있었지만 한번 더 정리
만약 input 이 비어있는 상태고 placeholder 가 있다면 placeholder 로 요소를 찾을 수는 있다. 다만 input 에 value 가 셋팅된 이후로는 이 방법으로는 요소를 찾을 수 없다.

```jsx
screen.getByPlaceholderText('날짜 범위를 선택해주세요')
```