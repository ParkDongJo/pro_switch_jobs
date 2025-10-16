# 첫번째 레슨
UI 상 Select 라고 여기고 9월 옆에 화살표를 타겟팅하여 선택하고자 했다. 하지만 실제 screen.logTestingPlaygroundURL() 를 찍어봤을 때, UI 상으로 보여지는 버튼이였고, 실제 select는 달란 하단에 위치해 있었다.

> UI 상에 보여지는 요소를 찾기보다는, 실제 DOM을 확인하고 그 DOM 구조를 바탕으로 요소를 찾자

![[Pasted image 20250915221009.png]]

# 두번째 레슨
select 요소를 타겟팅 할때도 역시 label 이 있어야, name 접근이 가능하다. 해당 ui 에서는 label 이 없는 상태이다. 이때도 여전히 웹접근성을 지킬려면 아래와 같은 방법들이 제공된다.

1. aria-label 사용 (가장 간단)
2. aria-labelledby 사용
	1. 이미 있는 다른 텍스트 요소에 id를 주고
	2. input 에 aria-labelledby 와 연결한다
3. 시각적으로 숨긴 레이블
	- visually-hidden 으로 숨긴다.
	- 이때 일반 display: none 이나 visibilty: hidden 으로 시각적으로 숨기면 안된다. 이때는 요소 자체가 완전히 제거되어 보조기술이 해당 요소를 인식할 수 없다.
```css
.visiually-hidden {
  overflow: hidden;
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
  white-space: nowrap;
  clip-path: inset(50%);
}
```


# 세번째 레슨
select 는 아래와 같이 찾을 수 있다.

```jsx
const monthSelect = screen.getAllByRole('combobox', { name: '월 선택' })
```

그리고 그 하위의 option 들은 아래와 같이 찾을 수 있다.

```jsx
await user.selectOptions(monthSelect, now.format('M월'));
```