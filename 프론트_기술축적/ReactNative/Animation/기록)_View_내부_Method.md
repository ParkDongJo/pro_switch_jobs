딥링크로 화면 진입 시 스크롤을 자동으로 지정하는 세션에 위치해달라는 요청사항이 있었다. 이 부분을 구현하기 위해 구현사항을 잘게 쪼개보면

- 세션에 대한 파라미터 받고 판단하기
- 화면 포커싱에 대한 이벤트 감지
- 위치시키고자 하는 세션 위치 값 가져오기
- 스크롤 원하는 위치로 내리기

여기서 애니메이션 작업에 대한 기록이니까 위치값 가져오기, 스크롤 원하는 위치로 내리기 등등을 기록해놓자


### Ref
위치값 가져오기, 스크롤을 내리기 등등은 이벤트를 실행시키고자 하는 행위는 그 View 자체에서 찾아야 한다. 아래와 같이 생각해보자

- View 의 위치 뽑기!
- ScrollView 의 Scroll 을 내리기!

그래서 각각 타겟팅하는 View 의 ref 를 뽑아둬야한다. 그래야 그 ref 를 통해서 View 에 내장되어 있는 기능들을 사용할 수 있다.

```jsx
const view1Ref = useRef(null);
const view2Ref = useRef(null);
const scrollViewRef = useRef(null);

<ScrollView
	onScroll={handleScroll}
	ref={scrollViewRef}
/>
	<SectionView ref={view1Ref}>
	</SectionView>
	<SectionView ref={view2Ref}>
	</SectionView>	
</ScrollView>

```


## Scrollview

**scrollTo**
스크롤을 내리고자 할 때, 아래와 같이 scrollTo 이라는 내장 함수를 아래와 같이 보내고자하는 x, y 값으로 셋팅하면 그 영역으로 스크롤이 된다.

```jsx
scrollViewRef.current?.scrollTo({ x: 0, y: y, animated: true })
```

**scrollToEnd**
스크롤을 Scrollview 의 끝단으로 내리는 내장 함수이다.

```jsx
scrollViewRef.current?.scrollToEnd();
```


## View

**measureLayout**
측정하고자 하는 View 의 위치 값을 뽑고자 한다면 measureLayout 내장 함수를 사용하면 된다. 측정함수가 내부적으로 3가지가 있다,

- measure
- measureInWindow
- measureLayout

그 중 measureLayout 는 첫 인자로 넘긴 상위 parnetsView 를 기준으로 targetView 의 위치를 측정한다. 즉
parnetsView 의 위치가 0, 0 이 되고 상대적인 위치로 targetView의 x, y 값을 가져온다.

```jsx
targetViewRef.current?.measureLayout(parnetsViewRef.current, (x, y: number) => { 
  scrollViewRef.current?.scrollTo({ x: x, y: y, animated: true });  
});
```

이때 스크롤 애니메이션 효과를 주고자 한다면 animated 를 true 로 주면 된다.

그 외 모든 View 에는 아래 내장 함수도 있다. 이름만 봐도 대략 기능을 알수 있으니 설명은 생략!!

- focus
- blur



# 결론!
View 의 Layout 에 대한 위치, 이동, UI 등등을 제어하고자 한다면,

- viewRef 를 먼저 뜨고
- viewRef.current 로 내장함수를 확인해본다.
- 그 중 내가 하고자 하는 기능이 있다면 그 기능을 활용하면 된다.

그런데! 사실 대 AI 시대가 다가오고있고, 이 정도의 기능은 AI가 대부분 작성을 해준다!. 개발자가 날로 먹는 시대..

즉, 중요한 건 개발자 본인이 무엇을 하겠다라는 정확한 그림을 그리고 있어야 한다. 앞으로는 기능 하나하나를 달달 외우기 보다 내가 하고자 하는 작업에 대한 세부 내용과 흐름을 표현하고 설계할수 있어야 하는것이 중요할 것 같다!