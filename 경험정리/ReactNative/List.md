
FlatList
- 한번에 렌더링 하지 않고 화면에 렌더링 될 요소만 렌더링 한다. 덕분에 성능이 최적화 되어 있다.
- 무한 스크롤을 구현하고자 할 시 사용하기에 좋다.

ScrollView
- 화면에 상관없이 모든 요소를 렌더링 시킨다. 때문에 성능을 저하시킬 수 있다.
- 다만, 렌더링 할 컴포넌트나 데이터가 한정적일 때, 줄바꿈이 일어날때 유용할 수 있다.


#### 중첩된 스크롤을 구현하고자 할 시

`주의사항`
Scrollview 안에 FlatList로 중첩된 스크롤을 구현하는 것은 FlatList의 성능상 이점을 전혀 가지고 가지 못한다.
이럴 때는 아래와 같이 가야한다.

`오해금물`
FlatList > FlatList 로 했을 시, 동일한 방향으로 스크롤 할때는 전혀 동작하지 않는다. 일부 블로그 글에서는 마치 이 것도 동일한 방향 스크롤이 가능할 것 처럼 표현해놨지만, 아래 설명과 같이 다른 방향으로 스크롤으로 구현될때 의미있는 구현방법이다.


##### 1 번째
기획적인 협의가 필요하다. 중첩된 스크롤을 할 시 `세로 > 가로` 또는 `가로 > 세로` 와 같이 서로 다른 방향으로 구현해야한다.
이때, FlatList > FlatList 구현으로 가야한다. 그러면 성능점 이점을 함께 가져가면서 충첩 스크롤 화면을 구현할 수 있다.

```typescript
<FlatList  
  data={[]}  
  renderItem={null}  
  ListEmptyComponent={  
    <>  
	    <View>  
		  <Text>Something to add above...</Text>  
		</View>
		<FlatList            
			data={datas}
			horizontal={true}  
			keyExtractor={item => item.key}  
			renderItem={ListItem}  
		/>
		<View>  
		  <Text>Something to add below...</Text>  
		</View>
    </>}
/>

```

위와 같이
- root FlatList 는 data = [] render = null 로 한다.
- children FlatList 는
	- root FlatList 의 ListEmptyComponent 에 넣어준다.
	- horizontal 을 true 로 준다

Flatlist 의 ListEmptyComponent 는 렌더링할 data 가 없을 보여주는 컴포넌트를 넣어주는것이 원래 사용법이다. 약간의 편법이라고 할 수 있다.



##### 2번째
동일한 방향으로 하고자 할 시에는 성능성 이점을 가지고 가기에는 한계점이 있다. 다만 그래도 성능을 고려한다면
- FlatList 안에 ScrollView 를 구현

하는 것이 나을 것이다. 하지만 굳이! 뿌려줘야할 컴포넌트가 많이 없다면

- Scrollview 안에 Scrollview 를 구현

하는 것도 괜찮다.

이때,
주의해야할 점이 있다! 이마저도 사실  일부 안드로이드 기기에서 동작하지 않을 수 있다. 경험상 Galaxy S8, S9 등등의 기기에서 자식 스크롤이 안되는 현상이 있었다.

이 현상을 해결하기 위해 아래와 같은 방법으로 풀고자 한적이 있다.

자식 스크롤 활성화 시 -> 부모 스크롤 해제
자식 스크롤 해제 시 -> 부모 스크롤 활성화

scrollEnabled 를 true, false 를 주면서 제어를 하는 것이다.

다만, 여기서 이런 시나리오가 가능하려면
스크롤을 활성화 / 해제 를 시키는 이벤트가 명확해야한다.

예를 들어

클릭 1 >
- 컴포넌트1가 보여지면서
- 부모 스크롤 활성화
클릭 2 >
- 컴포넌트 1은 닫히고
- 컴포넌트 2가 열리면서
- 자식 스크롤 활성화

이런 경우 교차 제어가 가능하다. 아래와 같이 

```typescript

const handleClickButtonOne = () => {
   setShowComponentOne(prev => !prev);
   setScrollEnabledOfParents(true);
}

const handleClickButtonTwo = () => {
   setShowComponentTwo(prev => !prev);
   setScrollEnabledOfParents(true);
}


<FlatList
  scrollEnabled={scrollEnabledOfParents}
  data={[]}  
  renderItem={null}
  ListEmptyComponent={  
    <>  
	    <Button onPress={handleClickButtonOne}>  
		  <Text>Open Component 1</Text>  
		</Button>
		<View isShow={showComponentOne}>
			...
	    <View>
		<Button onPress={handleClickButtonTwo}>  
		  <Text>Open Component 2</Text>  
		</Button>
		<View isShow={showComponentTwo}>
			<ScrollView
			    scrollEnabled={!scrollEnabledOfParents}            
				data={datas}
				horizontal={true}  
				keyExtractor={item => item.key}  
				renderItem={ListItem}  
			/>
		</View>
		<View>  
		  <Text>Something to add below...</Text>  
		</View>
    </>}
/>

```

handleClickButtonOne 또는 handleClickButtonTwo 같은 명확한 이벤트를 통해
scrollEnabledOfParents 의 값을 true <-> false 를 줘가면서 스크롤을 제어하는 것도 가능하다.

하지만 어디까지나 이것또한 편법일 뿐이고,

가장! 매끄럽고 생산적인 방법은.. 권장하는 방법을 쓰는것이다!