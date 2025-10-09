
# 📚 Zustand 완벽 이해 노트

## 🎯 **Zustand란?**

- **정의**: React 상태 관리 라이브러리 (독일어로 "상태"라는 뜻)
- **목적**: 여러 컴포넌트가 공유하는 전역 상태를 쉽고 효율적으로 관리
- **특징**:
    - 핵심 코드 단 42줄!
    - Context API 없이 동작
    - 선택적 리렌더링 지원

## 🚫 **왜 Context API를 안 쓰나?**

### Context API의 문제점

```javascript
// Context 사용 시 문제
const Context = React.createContext({ name: '', age: 0 });

// name만 바꿔도 age를 쓰는 컴포넌트도 리렌더링됨! 😱
```

**비유**: 아파트 101호가 전등 켜면 전체 아파트가 깜빡이는 것과 같음

## 🔧 **Zustand의 핵심 원리**

### 1️⃣ **클로저(Closure) - 비밀 금고**

```javascript
function create() {
  let state = { count: 0 };  // 외부에서 직접 접근 불가!
  
  return {
    getState: () => state,  // 금고 열어보기
    setState: (newState) => { state = newState }  // 금고 내용 바꾸기
  };
}
```

### 2️⃣ **발행/구독 패턴 - 유튜브 알림**

```javascript
const listeners = new Set();  // 구독자 명단

function subscribe(listener) {
  listeners.add(listener);  // 구독
}

function setState(newState) {
  state = newState;
  listeners.forEach(listener => listener());  // 구독자에게 알림!
}
```

## 📝 **코어 구현 분석**

### **기본 create 함수**

```javascript
function create(createState) {
  // 1. 클로저로 state 보호
  let state;
  
  // 2. 구독자 목록
  const listeners = new Set();
  
  // 3. 상태 변경 함수
  const setState = (partial) => {
    // 함수 또는 객체를 받을 수 있음
    const nextState = typeof partial === 'function' 
      ? partial(state) 
      : partial;
    
    if (nextState !== state) {
      state = Object.assign({}, state, nextState);
      listeners.forEach(listener => listener(state));
    }
  };
  
  // 4. 상태 조회 함수
  const getState = () => state;
  
  // 5. 구독 함수
  const subscribe = (listener) => {
    listeners.add(listener);
    return () => listeners.delete(listener);  // 구독 취소 함수 반환
  };
  
  // 6. 초기 상태 설정 (set, get, api 3개 인자 전달)
  state = createState(setState, getState, api);
  
  return { setState, getState, subscribe };
}
```

### **선택적 구독 (subscribeWithSelector)**

```javascript
const subscribeWithSelector = (listener, selector) => {
  // 구독 시점의 슬라이스 저장
  let currentSlice = selector(state);
  
  function listenerToAdd() {
    // state가 변경되면 새 슬라이스 추출
    const nextSlice = selector(state);
    
    if (currentSlice !== nextSlice) {
      currentSlice = nextSlice;
      listener(nextSlice);  // 변경됐을 때만 알림
    }
  }
  
  listeners.add(listenerToAdd);
};
```

## 🪝 **React Hook 구현**

### **useStore Hook의 핵심 구조**

```javascript
function createStoreHook(createState) {
  const api = create(createState);  // 스토어 생성
  
  const useStore = (selector) => {
    // 1. 강제 리렌더링 트릭
    const [, forceUpdate] = useReducer(c => c + 1, 0);
    
    // 2. 현재 슬라이스를 ref로 관리
    const currentSliceRef = useRef();
    currentSliceRef.current = selector(api.getState());
    
    // 3. 구독 설정 (마운트 시 1번만)
    useEffect(() => {
      const listener = () => {
        const nextSlice = selector(api.getState());
        
        if (currentSliceRef.current !== nextSlice) {
          currentSliceRef.current = nextSlice;
          forceUpdate();  // 변경 시에만 리렌더링!
        }
      };
      
      return api.subscribe(listener);
    }, []);
    
    return currentSliceRef.current;
  };
  
  return useStore;
}
```

## 💡 **핵심 개념 Q&A**

### Q1: selector는 어떻게 계속 다른 값을 꺼낼 수 있나?

**A**: `state` 변수가 setState로 계속 바뀜!

```javascript
// 시간 T1: state = {count: 0}
let currentSlice = selector(state);  // 0

// 시간 T2: setState로 state = {count: 5}
const nextSlice = selector(state);  // 5 (같은 selector, 다른 state!)
```

### Q2: createState는 왜 인자 개수가 다른가?

**A**: JavaScript는 필요한 인자만 받아도 OK!

```javascript
// Zustand가 3개 전달
state = createState(setState, getState, api);

// 사용자는 필요한 것만 받음
const createState = (set) => ({ ... });  // set만
const createState = (set, get) => ({ ... });  // set, get
const createState = (set, get, api) => ({ ... });  // 전부
```

### Q3: listener는 뭐야?

**A**: "상태 변경 시 실행할 콜백 함수"

```javascript
// 예시들
const logListener = (newValue) => console.log(newValue);
const rerenderListener = () => forceUpdate();
const storageListener = (state) => localStorage.setItem('state', state);
```

### Q4: 왜 무한 리렌더링이 안 일어나?

**A**: ref 업데이트는 리렌더링을 안 일으킴!

```javascript
// Effect 1: ref만 업데이트 (리렌더링 X)
useEffect(() => {
  currentSliceRef.current = newSlice;  // ref 업데이트는 리렌더링 안함
});

// Effect 2: 구독 설정 (마운트 시 1번)
useEffect(() => {
  const listener = () => {
    if (변경됨) forceUpdate();  // 진짜 변경 시에만!
  };
  return api.subscribe(listener);
}, []);  // deps가 []라서 1번만 실행
```

## 🎮 **실제 사용법**

```javascript
// 1. 스토어 생성
const useStore = create((set) => ({
  bears: 0,
  increaseBears: () => set(state => ({ bears: state.bears + 1 })),
  resetBears: () => set({ bears: 0 })
}));

// 2. 컴포넌트에서 사용
function BearCounter() {
  // 필요한 부분만 선택
  const bears = useStore(state => state.bears);
  return <h1>{bears} 마리의 곰</h1>;
}

function Controls() {
  // 액션만 선택
  const increaseBears = useStore(state => state.increaseBears);
  return <button onClick={increaseBears}>곰 추가</button>;
}
```

## 🎯 **핵심 정리**

1. **Zustand = 스마트한 우체국**
    
    - 각자의 우편함(state)
    - 편지 도착(setState) → 해당 구독자만 알림
    - 다른 사람은 방해받지 않음
2. **주요 트릭**
    
    - `forceUpdate`: useReducer로 강제 리렌더링
    - `useRef`: 리렌더링 없이 값 보관
    - `클로저`: state를 외부에서 보호
3. **장점**
    
    - 필요한 컴포넌트만 리렌더링
    - Provider 지옥 없음
    - 작은 번들 사이즈
    - React 없이도 사용 가능

이제 Zustand의 전체 동작 원리를 완벽히 이해했습니다! 🎉