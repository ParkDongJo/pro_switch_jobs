객체 State([[학습)_객체_State_업데이트하기]]) 와 마찬가지로 새 배열을 생성한 뒤에 새 배열을 state 로 갱신해야한다.

배열을 업데이트할 때마다 _새_ 배열을 state 설정 함수에 전달해야 한다
그래서 새 배열로 전달하는 내장함수를 사용한다.

|     | 사용 X (배열을 변경)               | 사용 (새 배열을 반환)               |
| --- | --------------------------- | --------------------------- |
| 추가  | `push`, `unshift`           | `concat`, `[...arr]` 전개 연산자 |
| 제거  | `pop`, `shift`, `splice`    | `filter`, `slice`           |
| 교체  | `splice`, `arr[i] = ...` 할당 | `map`                       |
| 정렬  | `reverse`, `sort`           | 배열을 복사한 이후 처리               |
# 항목 추가

push 역할
```typescript
setArtists( // 아래의 새로운 배열로 state를 변경합니다.  

[  

...artists, // 기존 배열의 모든 항목에,  

{ id: nextId++, name: name } // 마지막에 새 항목을 추가합니다.  

]  

);
```

unshift 역할
```typescript
setArtists([  

{ id: nextId++, name: name }, // 추가할 항목을 앞에 배치하고,  

...artists // 기존 배열의 항목들을 뒤에 배치합니다.  

]);
```

# 항목 추가 2 (특정 지점)

```typescript
    const nextArtists = [
      // 삽입 지점 이전 항목
      ...artists.slice(0, insertAt),
      // 새 항목
      { id: nextId++, name: name },
      // 삽입 지점 이후 항목
      ...artists.slice(insertAt)
    ];
    setArtists(nextArtists);

  }
```


# 항목 제거
```typescript
setArtists(  

artists.filter(a => a.id !== artist.id)  

);
```


# 배열 반환

```typescript
    const nextShapes = shapes.map(shape => {
      if (shape.type === 'square') {
        // 변경시키지 않고 반환합니다.
        return shape;
      } else {
        // 50px 아래로 이동한 새로운 원을 반환합니다.
        return {
          ...shape,
          y: shape.y + 50,
        };
      }
    });
    // 새로운 배열로 리렌더링합니다.
    setShapes(nextShapes);
```


# 배열 항목 교체

```typescript
    const nextCounters = counters.map((c, i) => {
      if (i === index) {
        // 클릭된 counter를 증가시킵니다.
        return c + 1;
      } else {
        // 변경되지 않은 나머지를 반환합니다.
        return c;
      }
    });
    setCounters(nextCounters);
```


# 배열 정렬 / 역정렬

```typescript
    const nextList = [...list];
    nextList.reverse();
    setList(nextList);
```

**배열을 복사하더라도 배열 _내부_ 에 기존 항목을 직접 변경해서는 안된다**. 이는 얕은 복사이기 때문에 복사한 새 배열에는 원본 배열과 동일한 항목이 포함된다.


# 배열 내 객체 변경

```typescript
setMyList(myList.map(artwork => {  

if (artwork.id === artworkId) {  

// 변경된 *새* 객체를 만들어 반환합니다.  

return { ...artwork, seen: nextSeen };  

} else {  

// 변경시키지 않고 반환합니다.  

return artwork;  

}  

}));
```

마찬가지로 여기서도 (참고: [[학습)_객체_State_업데이트하기]])
Immer를 사용하면 **`artwork.seen = nextSeen`과 같이 변경해도 괜찮다

```typescript
updateMyTodos(draft => {  

const artwork = draft.find(a => a.id === artworkId);  

artwork.seen = nextSeen;  

});
```
내부적으로 Immer는 항상 `draft`에서 수행한 변경 사항에 따라 처음부터 다음 state를 구성한다
state를 변경하지 않고도 이벤트 핸들러를 매우 간결하게 유지할 수 있다.