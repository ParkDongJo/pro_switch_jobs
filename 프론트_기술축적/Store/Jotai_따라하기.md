# 기본 컨셉

Context API 를 기반으로 store 관리, 기존의 Context 성능적 이슈를 보완하는 식으로 커스텀 했음

# Store 생성

Provider 내부에서 createStore() 로 생성해줌

createStore는 최초 buildStore 를 통해 생성됨

> src/react/Provider.ts
> src/vanilla/store.ts
> src/vanilla/internals.ts

```typescript
const getInternalBuildingBlocks = (store: Store): Readonly<BuildingBlocks> => {
  const buildingBlocks = buildingBlockMap.get(store)!;
  if (import.meta.env?.MODE !== "production" && !buildingBlocks) {
    throw new Error(
      "Store must be created by buildStore to read its building blocks"
    );
  }
  return buildingBlocks;
};

function buildStore(...buildArgs: Partial<BuildingBlocks>): Store {
  const store = {
    get(atom) {
      const storeGet = getInternalBuildingBlocks(store)[21];
      return storeGet(store, atom);
    },
    set(atom, ...args) {
      const storeSet = getInternalBuildingBlocks(store)[22];
      return storeSet(store, atom, ...args);
    },
    sub(atom, listener) {
      const storeSub = getInternalBuildingBlocks(store)[23];
      return storeSub(store, atom, listener);
    },
  } as Store;

  const buildingBlocks = (
    [
      // store state
      new WeakMap(), // atomStateMap
      new WeakMap(), // mountedMap
      new WeakMap(), // invalidatedAtoms
      new Set(), // changedAtoms
      new Set(), // mountCallbacks
      new Set(), // unmountCallbacks
      {}, // storeHooks
      // atom interceptors
      atomRead,
      atomWrite,
      atomOnInit,
      atomOnMount,
      // building-block functions
      ensureAtomState,
      flushCallbacks,
      recomputeInvalidatedAtoms,
      readAtomState,
      invalidateDependents,
      writeAtomState,
      mountDependencies,
      mountAtom,
      unmountAtom,
      setAtomStateValueOrPromise,
      storeGet,
      storeSet,
      storeSub,
      undefined,
    ] satisfies BuildingBlocks
  ).map((fn, i) => buildArgs[i] || fn) as BuildingBlocks;

  buildingBlockMap.set(store, Object.freeze(buildingBlocks));
  return store;
}
```

여기서 아래 2가지가 가장 중요하다고 생각했음

- 상태 읽기
- 상태 쓰기

코드를 보면 store 에 정의된 3가지 get, set, sub 라는 인터페이스가 있고,
이 각각의 인터페이스는 buildingBlocks에서 21, 22, 23 인덱스로 각각 빼오고 있다.

buildingBlocks에서 에서
21 -> storeGet
22 -> storeSet
23 -> storeSub

이고,

```typescript
const storeGet: StoreGet = (store, atom) => {
  const readAtomState = getInternalBuildingBlocks(store)[14];
  return returnAtomValue(readAtomState(store, atom)) as any;
};

const storeSet: StoreSet = (store, atom, ...args) => {
  const buildingBlocks = getInternalBuildingBlocks(store);
  const flushCallbacks = buildingBlocks[12];
  const recomputeInvalidatedAtoms = buildingBlocks[13];
  const writeAtomState = buildingBlocks[16];
  try {
    return writeAtomState(store, atom, ...args) as any;
  } finally {
    recomputeInvalidatedAtoms(store);
    flushCallbacks(store);
  }
};
```

storeGet 에서는 buildingBlocks에서 14 인덱스를 뽑고있음 -> readAtomState
storeSet 에서는 buildingBlocks에서 16 인덱스를 뽑고있음 -> writeAtomState

readAtomState 과 writeAtomState 를 살펴볼 필요 있다.

readAtomState를 먼저 살펴보자

```typescript
const readAtomState: ReadAtomState = (store, atom) => {
  const buildingBlocks = getInternalBuildingBlocks(store);
  const mountedMap = buildingBlocks[1];
  const invalidatedAtoms = buildingBlocks[2];
  const changedAtoms = buildingBlocks[3];
  const storeHooks = buildingBlocks[6];
  const atomRead = buildingBlocks[7];
  const ensureAtomState = buildingBlocks[11];
  const flushCallbacks = buildingBlocks[12];
  const recomputeInvalidatedAtoms = buildingBlocks[13];
  const readAtomState = buildingBlocks[14];
  const writeAtomState = buildingBlocks[16];
  const mountDependencies = buildingBlocks[17];
  const atomState = ensureAtomState(store, atom);

  // 캐시 확인
  if (isAtomStateInitialized(atomState)) {
    // ...생략
    return atomState;
  }
  //------ 설정 시작 -----
  atomState.d.clear();
  let isSync = true;
  function mountDependenciesIfAsync() {
    // ...생략
  }
  function getter<V>(a: Atom<V>) {
    // ...생략
    try {
      return returnAtomValue(aState);
    } finally {
      // ...생략
      if (!isSync) {
        mountDependenciesIfAsync();
      }
    }
  }
  let controller: AbortController | undefined;
  let setSelf: ((...args: unknown[]) => unknown) | undefined;
  const options = {
    get signal() {
      // ...생략
      return controller.signal;
    },
    get setSelf() {
      // ...생략
      return setSelf;
    },
  };
  //------ 설정 끝 -----
  const prevEpochNumber = atomState.n;
  try {
    const valueOrPromise = atomRead(store, atom, getter, options as never);
    setAtomStateValueOrPromise(store, atom, valueOrPromise);
    if (isPromiseLike(valueOrPromise)) {
      registerAbortHandler(valueOrPromise, () => controller?.abort());
      valueOrPromise.then(mountDependenciesIfAsync, mountDependenciesIfAsync);
    }
    storeHooks.r?.(atom);
    return atomState;
  } catch (error) {
    delete atomState.v;
    atomState.e = error;
    ++atomState.n;
    return atomState;
  } finally {
    isSync = false;
    if (
      prevEpochNumber !== atomState.n &&
      invalidatedAtoms.get(atom) === prevEpochNumber
    ) {
      invalidatedAtoms.set(atom, atomState.n);
      changedAtoms.add(atom);
      storeHooks.c?.(atom);
    }
  }
};
```

위 코드를 최대한 생략해서 보면, 크게는 3가지 영역으로 나뉜다.

첫번째, 캐시여부를 확인하고 캐시되어 있는 atomState 를 반환
두번째, atomRead() 로 넘길 getter, options 만들기
세번째, atomRead() 호출 이후 의존성 관리
네번째, storeHooks() 에 read 리스너 호출

여기서 의존성이라는 것에 대해 이해할 필요가 있다.

Atom 의 의존성은 각 atom 이 변경되었을 때, 연결된 atom 들을 추적해서 의존하는 모든 atom 들을 자동으로 업데이트 하기 위해 관리된다.

이는 예를 들어, recoil 에서 atom 에 파생되는 selector나 atomFamily 같은 개념들이 똑같이 jotai 에도 적용되는 것으로 보인다.

```typescript
type AtomState<Value = AnyValue> = {
  /**
   * Map of atoms that the atom depends on.
   * The map value is the epoch number of the dependency.
   */
  readonly d: Map<AnyAtom, EpochNumber>;
  //...생략
};

type Mounted = {
  //...생략
  /** Set of mounted atoms that the atom depends on. */
  readonly d: Set<AnyAtom>;
  /** Set of mounted atoms that depends on the atom. */
  readonly t: Set<AnyAtom>;
  //...생략
};
```

> src/vanilla/internals.ts

```typescript
function readAtomState() {
  // ...생략

  try {
    const valueOrPromise = atomRead(store, atom, getter, options as never)
    setAtomStateValueOrPromise(store, atom, valueOrPromise)
  // ...생략
  } catch () {

  }
}
```

atom의 read 함수가 실행되면서 getter 함수를 통해 다른 atom들을 읽습니다.

주입 과정이 좀 복잡하지만, 차근차근 따라가보면

```typescript
const atomRead: AtomRead = (_store, atom, ...params) => atom.read(...params);
```

atom 은 아래와 같은 구조로 되어 있다.

> src/vanilla/atom.ts

```typescript
export interface Atom<Value> {
  toString: () => string;
  read: Read<Value>;
  //...생략
}

type Read<Value, SetSelf = never> = (
  get: Getter,
  options: { readonly signal: AbortSignal; readonly setSelf: SetSelf }
) => Value;
```

보면 getter 는 get 이라는 메서드 명으로 다시 주입되는데
아래에 atom 내부에 read 에 주입되는 코드를 를 보면

```typescript
export function atom<Value, Args extends unknown[], Result>(
  read?: Value | Read<Value, SetAtom<Args, Result>>,
  write?: Write<Args, Result>
) {
  const key = `atom${++keyCount}`;
  const config = {
    toString() {
      return import.meta.env?.MODE !== "production" && this.debugLabel
        ? key + ":" + this.debugLabel
        : key;
    },
  } as WritableAtom<Value, Args, Result> & { init?: Value | undefined };
  if (typeof read === "function") {
    config.read = read as Read<Value, SetAtom<Args, Result>>;
  } else {
    config.init = read;
    config.read = defaultRead;
    config.write = defaultWrite as unknown as Write<Args, Result>;
  }
  if (write) {
    config.write = write;
  }
  return config;
}

function defaultRead<Value>(this: Atom<Value>, get: Getter) {
  return get(this);
}
```

별일이 없다면 defaultRead 가 최초로 주입이되고 defaultRead는 get을 호출하면서, atom 을 주입하고 있다.

readAtomState 호출되면 atom.read 가 실행되면서 getter 가 실행 되고, 이때 getter 의 내부 로직들이 수행되는데

이때 getter 안에

```typescript
function readAtomState() {
  // ...생략
  function getter<V>(a: Atom<V>) {
    if (a === (atom as AnyAtom)) {
      const aState = ensureAtomState(store, a);
      if (!isAtomStateInitialized(aState)) {
        if (hasInitialValue(a)) {
          setAtomStateValueOrPromise(store, a, a.init);
        } else {
          // NOTE invalid derived atoms can reach here
          throw new Error("no atom init");
        }
      }
      return returnAtomValue(aState);
    }
    // a !== atom
    const aState = readAtomState(store, a);
    try {
      return returnAtomValue(aState);
    } finally {
      // 의존성 기록
      atomState.d.set(a, aState.n);
      if (isPendingPromise(atomState.v)) {
        addPendingPromiseToDependency(atom, atomState.v, aState);
      }
      mountedMap.get(a)?.t.add(atom);
      if (!isSync) {
        mountDependenciesIfAsync();
      }
    }
  }
  // ...생략
}
```

atomState.d.set(a) 와 mountedMap.get(a)?.t.add(atom) 를 통해서 의존성을 기록하는 것으로 보인다.

readAtomState 안에서 readAtomState 가 호출 되고 있는데, 재귀함수로 의존성을 파악하고 있다.

그렇다면, 기저조건은

```typescript
function readAtomState() {
  // ...생략
  if (isAtomStateInitialized(atomState)) {
    // If the atom is mounted, we can use cached atom state.
    // because it should have been updated by dependencies.
    // We can't use the cache if the atom is invalidated.
    if (mountedMap.has(atom) && invalidatedAtoms.get(atom) !== atomState.n) {
      return atomState;
    }
    // Otherwise, check if the dependencies have changed.
    // If all dependencies haven't changed, we can use the cache.
    if (
      Array.from(atomState.d).every(
        ([a, n]) =>
          // Recursively, read the atom state of the dependency, and
          // check if the atom epoch number is unchanged
          readAtomState(store, a).n === n
      )
    ) {
      return atomState;
    }
  }
  //...생략
}
```

두번째 기저조건

```typescript
function readAtomState() {
  // ...생략
  function getter<V>(a: Atom<V>) {
    if (a === (atom as AnyAtom)) {
      const aState = ensureAtomState(store, a);
      if (!isAtomStateInitialized(aState)) {
        if (hasInitialValue(a)) {
          setAtomStateValueOrPromise(store, a, a.init);
        } else {
          // NOTE invalid derived atoms can reach here
          throw new Error("no atom init");
        }
      }
      return returnAtomValue(aState);
    }
  }
  // ...생략
}
```

등등이 있다.
