# Babel 의 역할

Babel 이 ES6 최신 문법을 ES5 문법으로 변환해주는 트랜스파일링(변환) 작업을 해결해준다. 점점 나아가서는 React의 JSX, Typescript 정적타입 언어 등등의 다양한 변환을 제공하면서 프론트에 있어서는 필수적인 기술로 자리 잡았다. [[프론트_기술축적/Builder/Index|Index]]

# Babel 의 변환 과정

### 파싱단계

소스코드를 AST(Abstract Syntax Tree)로 변환한다.

### 변환단계

생성된 AST(Abstract Syntax Tree)를 순회하면서, ES5 문법으로 변환해준다.

```javascript
() => 1

// 변환
function() { return 1; }
```

```jsx
<div>Hello</div>;

// 변환
React.createElement("div", null, "Hello");
```

### 코드 생성

변환된 AST를 다시 문자열 형태의 JavaScript 코드로 변환한다.

# React Compiler 내부코드

[Babel/ReactCompilerBablePlugin](compiler/packages/babel-plugin-react-compiler/src/Babel/RunReactCompilerBabelPlugin.ts)
React 프로젝트 내부에는 React Compiler Babel Plugin 코드가 있다. 특히 runBabelPluginReactCompiler() 함수는 이 플러그인이 실행될 때 동작하는 함수로 보인다.

함수의 코드는 아래와 같다.

```typescript
/**
 * Copyright (c) Meta Platforms, Inc. and affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 */

import type * as BabelCore from "@babel/core";
import { transformFromAstSync } from "@babel/core";
import * as BabelParser from "@babel/parser";
import invariant from "invariant";
import type { PluginOptions } from "../Entrypoint";
import BabelPluginReactCompiler from "./BabelPlugin";

export const DEFAULT_PLUGINS = ["babel-plugin-fbt", "babel-plugin-fbt-runtime"];
export function runBabelPluginReactCompiler(
  text: string,
  file: string,
  language: "flow" | "typescript",
  options: PluginOptions | null,
  includeAst: boolean = false
): BabelCore.BabelFileResult {
  const ast = BabelParser.parse(text, {
    sourceFilename: file,
    plugins: [language, "jsx"],
    sourceType: "module",
  });
  const result = transformFromAstSync(ast, text, {
    ast: includeAst,
    filename: file,
    highlightCode: false,
    retainLines: true,
    plugins: [
      [BabelPluginReactCompiler, options],
      "babel-plugin-fbt",
      "babel-plugin-fbt-runtime",
    ],
    sourceType: "module",
    configFile: false,
    babelrc: false,
  });
  invariant(
    result?.code != null,
    `Expected BabelPluginReactForget to codegen successfully, got: ${result}`
  );
  return result;
}
```

### BabelParser.parse

BabelParser.parse로 파싱을해서 AST 를 만든다. @babel/parser 에서 Parser 를 가져왔고, language로 jsx 를 넘겼다.

### transformFromAstSync

@babel/core 에서 transformFromAstSync 함수를 꺼내왔고 ast 를 넘기고 있다. 이때 plugin 으로 BabelPluginReactCompiler 를 넘기고 있다.

# [[React Compiler]]

애플리케이션을 최적화하기 위해 코드를 자동으로 메모이제이션(Memoization)을 한다. useMemo, useCallback, React.memo 와 같은 최적화를 Compiler 가 자동으로 해준다.

#### 렌더링 최적화

상태 변경 시 앱에서 관련된 부분만 리렌더링되도록 수동 메모이제이션과 동등한 기능을 자동으로 적용합니다.

```jsx
function FriendList({ friends }) {
  const onlineCount = useFriendOnlineCount();

  if (friends.length === 0) {
    return <NoFriends />;
  }

  return (
    <div>
      <span>{onlineCount} online</span>
      {friends.map((friend) => (
        <FriendListCard key={friend.id} friend={friend} />
      ))}
      <MessageButton />
    </div>
  );
}
```

```javascript
import { c as _c } from "react/compiler-runtime";

function FriendList(t0) {
  const $ = _c(9);

  const { friends } = t0;

  const onlineCount = useFriendOnlineCount();

  if (friends.length === 0) {
    let t1;

    if ($[0] === Symbol.for("react.memo_cache_sentinel")) {
      t1 = <NoFriends />;

      $[0] = t1;
    } else {
      t1 = $[0];
    }

    return t1;
  }

  let t1;

  if ($[1] !== onlineCount) {
    t1 = <span>{onlineCount} online</span>;

    $[1] = onlineCount;

    $[2] = t1;
  } else {
    t1 = $[2];
  }

  // friends 배열이 변경되었는지 확인하는 메모이제이션 로직
  // 이 부분이 useMemo(() => friends.map(...), [friends])와 동등한 기능
  let t2;

  if ($[3] !== friends) {
    // friends 참조가 변경된 경우에만 map 재실행
    t2 = friends.map(_temp);

    $[3] = friends; // 이전 friends 참조 저장
    $[4] = t2; // 계산된 결과(JSX 배열) 캐싱
  } else {
    // friends 참조가 동일하면 캐시된 결과 재사용
    t2 = $[4];
  }

  let t3;

  if ($[5] === Symbol.for("react.memo_cache_sentinel")) {
    t3 = <MessageButton />;

    $[5] = t3;
  } else {
    t3 = $[5];
  }

  let t4;

  if ($[6] !== t1 || $[7] !== t2) {
    t4 = (
      <div>
        {t1}

        {t2}

        {t3}
      </div>
    );

    $[6] = t1;

    $[7] = t2;

    $[8] = t4;
  } else {
    t4 = $[8];
  }

  return t4;
}

function _temp(friend) {
  return <FriendListCard key={friend.id} friend={friend} />;
}
```

## React Compiler의 메모이제이션 동작 원리

### 핵심 개념: 참조 비교 (Reference Equality)

```javascript
if ($[3] !== friends) {
  t2 = friends.map(_temp); // 재계산
  $[3] = friends; // 현재 참조 저장
  $[4] = t2; // 결과 캐싱
} else {
  t2 = $[4]; // 캐시된 결과 재사용
}
```

### 왜 이렇게 동작하는가?

#### 1. **참조 기반 최적화 (Reference-based Optimization)**

```javascript
// 시나리오 1: friends 배열 참조가 변경되지 않음
const [friends, setFriends] = useState([{ id: 1 }, { id: 2 }]);

// 첫 렌더: friends.map() 실행 → 결과를 $[4]에 캐싱
// 두번째 렌더: onlineCount만 변경
// → $[3] !== friends는 false (같은 참조)
// → $[4]에 캐시된 결과 재사용 ✅ (map 실행 안함!)
```

```javascript
// 시나리오 2: friends 배열 참조가 변경됨
setFriends([{ id: 1 }, { id: 2 }, { id: 3 }]); // 새로운 배열 객체!

// → $[3] !== friends는 true (다른 참조)
// → friends.map(_temp) 재실행 ✅
// → 새로운 결과를 $[4]에 캐싱
```

#### 2. **수동 메모이제이션과의 비교**

```jsx
// 수동 메모이제이션 (개발자가 직접 작성)
function FriendList({ friends }) {
  const onlineCount = useFriendOnlineCount();

  // friends가 변경될 때만 map 재실행
  const friendCards = useMemo(
    () =>
      friends.map((friend) => (
        <FriendListCard key={friend.id} friend={friend} />
      )),
    [friends] // 의존성 배열
  );

  return (
    <div>
      <span>{onlineCount} online</span>
      {friendCards}
      <MessageButton />
    </div>
  );
}
```

```javascript
// React Compiler가 자동 생성한 코드 (위와 동일한 효과)
if ($[3] !== friends) {
  // useMemo의 의존성 검사와 동일
  t2 = friends.map(_temp);
  $[3] = friends;
  $[4] = t2;
} else {
  t2 = $[4]; // useMemo의 캐시된 값 반환과 동일
}
```

### 실제 최적화 효과

#### Case 1: `onlineCount`만 변경되는 경우

```
1. onlineCount: 5 → 10 변경
2. friends 참조는 동일
3. React Compiler의 동작:
   - $[1] !== onlineCount → true
     → t1 = <span>10 online</span> 재생성 ✅

   - $[3] !== friends → false (동일한 참조!)
     → t2 = $[4] 캐시 재사용 ✅ (map 실행 안함!)

   - t1이 변경되었으므로 최종 div만 재생성
   - MessageButton은 캐시 재사용 ($[5])
```

**결과**: `friends.map()`이 실행되지 않아 **성능 최적화** 달성!

#### Case 2: `friends`가 변경되는 경우

```
1. friends: [{id: 1}] → [{id: 1}, {id: 2}] 변경
2. 새로운 배열 객체 생성 (다른 참조)
3. React Compiler의 동작:
   - $[3] !== friends → true (다른 참조!)
     → t2 = friends.map(_temp) 재실행 ✅
     → 새로운 FriendListCard 배열 생성

   - 최종 div 재생성
```

**결과**: friends가 실제로 변경되었으므로 map을 **정확하게 재실행**!

### 핵심 포인트

1. **"관련된 부분만 리렌더링"의 의미**

   - `onlineCount`만 변경 → `friends.map()` 실행 안함 (관련 없음)
   - `friends`만 변경 → `<span>` 재사용, `map()` 재실행 (관련 있음)

2. **참조 비교의 효율성**

   - `$[3] !== friends`는 O(1) 연산 (매우 빠름)
   - 배열 내용 비교가 아닌 참조만 비교
   - JavaScript의 메모리 주소 비교

3. **캐시 배열 `$`의 역할**

   ```javascript
   const $ = _c(9); // 9개의 슬롯을 가진 캐시 배열

   // $[0]: <NoFriends /> 캐시
   // $[1]: onlineCount 이전 값
   // $[2]: <span> 엘리먼트 캐시
   // $[3]: friends 이전 참조
   // $[4]: friends.map() 결과 캐시
   // $[5]: <MessageButton /> 캐시
   // $[6], $[7], $[8]: 최종 div 관련 캐시
   ```

### 정리

`friends`가 변경되면 `t2 = friends.map(_temp)`를 다시 실행하는 것은 **정확히 필요한 시점에만 재계산**하기 위함입니다.

- ✅ `friends` 참조가 다르면 → 내용이 변경되었을 가능성이 높음 → 재계산 필요
- ✅ `friends` 참조가 같으면 → 내용이 동일 → 이전 결과 재사용

이것이 "관련된 부분만 리렌더링"의 핵심이며, React Compiler는 이를 자동으로 처리해줍니다!

# 컴파일러를 적용하려면

컴파일러는 React의 규칙을 따르는 함수 컴포넌트와 Hook을 컴파일하는 것을 목표로 설계되었다. 이러한 규칙을 위반하는 코드도 해당 컴포넌트나 Hook을 건너뛰는 방식으로 처리할 수 있다.

> "use no memo"
> "use no memo"는 React 컴파일러에 의해 컴파일되지 않도록 컴포넌트와 Hooks를 선택적으로 제외할 수 있는 임시 탈출구입니다. 이 지시어는 "use client"와 같이 장기적으로 사용하지 않을 임시방편이다.
> 이 지시어는 필요한 경우가 아니면 사용을 권장하지 않는다. 한 번 컴포넌트나 Hook을 제외하면 해당 지시어가 제거될 때까지 영구적으로 컴파일에서 제외한다

# 참고자료

https://ko.react.dev/learn/react-compiler
https://developers.kakaomobility.com/docs/techblogs/react-compiler/
https://helloinyong.tistory.com/365
