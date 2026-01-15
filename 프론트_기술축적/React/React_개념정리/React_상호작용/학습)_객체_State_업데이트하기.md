State 로 객체를 업데이트하고 싶을 때는 새로운 객체를 생성하여 (또는 기존 객체의 복사본을 만들어), state가 복사본을 사해라

```tsx
setPerson({  
	...person, // 이전 필드를 복사  
	firstName: e.target.value // 새로운 부분은 덮어쓰기  
});
```

`...` 전개 문법은 “얕다”는 점을 알아두세요. 이것은 한 레벨 깊이의 내용만 복사한다.

이전의 state 를 활용한다면, 아래와 같이 prevState 를 활용한다. [[학습)_State_업데이트_큐]]
```tsx
setPerson(prev => ({  
	...prev, // 이전 필드를 복사  
	firstName: e.target.value // 새로운 부분은 덮어쓰기  
}));
```

state 로 다루는 객체의 구조가 2차 이상의 구조로 들어갈 때, 아래와 같이!
```tsx
setPerson({  
	...person, // 다른 필드 복사  
	artwork: { // artwork 교체  
		...person.artwork, // 동일한 값 사용  
		city: 'New Delhi' // 하지만 New Delhi!  
	}
});
```

하지만 위의 구조가 복잡하다고 느낀다면 Immer 를 사용한다.

```tsx
import { useImmer } from 'use-immer';

const [person, updatePerson] = useImmer({
	name: 'Niki de Saint Phalle',
	artwork: {
		title: 'Blue Nana',
		city: 'Hamburg',
		image: 'https://i.imgur.com/Sd1AgUOm.jpg',
	}
});

updatePerson(draft => {  
	draft.artwork.city = 'Lagos';  
});
```

> Immer가 제공하는 `draft`는 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)라고 하는 아주 특별한 객체 타입으로, 당신이 하는 일을 “기록” 합니다. 객체를 원하는 만큼 자유롭게 변경할 수 있는 이유죠! Immer는 내부적으로 `draft`의 어느 부분이 변경되었는지 알아내어, 변경사항을 포함한 완전히 새로운 객체를 생성합니다.




> **React는 state를 ‘불변한 스냅샷’으로 다루는 것을 전제로 설계되었다**

- **디버깅이 쉬워짐**
    state를 변경하지 않으면 렌더링 사이의 상태 변화를 명확히 추적할 수 있다
- **성능 최적화에 유리함**
    prevState === nextState 같은 빠른 비교로 변경 여부를 판단할 수 있다
- **미래 React 기능과의 호환성**
    Concurrent Rendering 등 새로운 기능은 state가 스냅샷처럼 유지된다는 가정에 의존 한다
- **구현이 단순해짐**
    프록시나 특별한 감시 로직 없이도 어떤 객체든 state로 사용할 수 있습니다.


결론적으로, **state를 불변으로 유지하는 습관은 지금뿐 아니라 미래의 React 생태계까지 고려한 선택**