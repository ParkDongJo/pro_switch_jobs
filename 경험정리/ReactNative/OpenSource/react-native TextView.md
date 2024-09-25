# 주요
일단! 가장 간단할수 있는 TextView 를 살펴보면서, Native 코드와 JavaScript 코드가 어떻게 연결이 되는지 서로 통신을 주고받는지를 확인하고자 한다.

그 전에 어떤 식으로 코드가 구성되어있는지 살펴본다.


# 

코드를 타고타고 가보자

먼저 Text 에서 아래와 같이 NativeText 또는 NativeVirtualText를 몇몇 분기문과 함께 뱉어주고 있다.
```javascript
// packages/react-native/Libraries/Text.js
...

/* 타고 가보기! */
import {NativeText, NativeVirtualText} from './TextNativeComponent';
...

const Text: React.AbstractComponent<TextProps, TextForwardRef> = React.forwardRef(() => {

	nativeText = (
		<NativeText />
	)
	...
	return (
		<TextAncestor.Provider value={true}>{nativeText}</TextAncestor.Provider>
	)
});


```

Native UI와 연결될 것 처럼 보이는 컴포넌트는 NativeText, NativeVirtualText 이기 때문에 타고 내려가본다.

```javascript
// packages/react-native/Libraries/Text/TextNativeComponent.js

/* 타고 가보기! */
import createReactNativeComponentClass from '../Renderer/shims/createReactNativeComponentClass';

...

export const NativeText: HostComponent<NativeTextProps> =  
  (createReactNativeComponentClass('RCTText', () =>  
    createViewConfig(textViewConfig),  
  ): any);  
  
export const NativeVirtualText: HostComponent<NativeTextProps> =  
  !global.RN$Bridgeless && !UIManager.hasViewManagerConfig('RCTVirtualText')  
    ? NativeText  
    : (createReactNativeComponentClass('RCTVirtualText', () =>  
        createViewConfig(virtualTextViewConfig),  
      ): any);

```

createReactNativeComponentClass() 라는 함수를 통해서, name 으로 `RCTText`, `RCTVirtualText` 을 넘겨주고 있다. 아무래도 Native UI 에 대한 Naitve 코드 파일 이름은  `RCTText`, `RCTVirtualText` 와 연관이 있을 것으로 추측된다.

```javascript
// packages/react-native/Libraries/Render/shims/createReactNativeComponentClass.js

/* 타고 가보기! */
import {ReactNativeViewConfigRegistry} from 'react-native/Libraries/ReactPrivate/ReactNativePrivateInterface';
const {register} = ReactNativeViewConfigRegistry;

...

const createReactNativeComponentClass = function (  
  name: string,  
  callback: () => ViewConfig,  
): string {  
  return register(name, callback);  
};
```

createReactNativeComponentClass() 내부를 드려다보면, register() 함수를 만날수 있다. 아무래도 이 함수가 Native UI 와 관련된 Component 를 메모리에 등록을 해두는 함수로 보인다.


```javascript
// packages/react-native/Libraries/ReactPrivate/ReactNativeInterface.js

module.exports = {  
  ...

  /* 타고 가보기! */
  get ReactNativeViewConfigRegistry(): ReactNativeViewConfigRegistry {  
    return require('../Renderer/shims/ReactNativeViewConfigRegistry');  
  },  

  ...
}
```



```javascript
//packages/react-naitve/Libraries/shims/ReactNativeViewConfigRegistry.js

/**  
 * Registers a native view/component by name. 
 * A callback is provided to load the view config from UIManager. 
 * The callback is deferred until the view is actually rendered. 
 * */
 export function register(name: string, callback: () => ViewConfig): string {  
  invariant(  
    !viewConfigCallbacks.has(name),  
    'Tried to register two views with the same name %s',  
    name,  
  );  
  invariant(  
    typeof callback === 'function',  
    'View config getter callback for component `%s` must be a function (received `%s`)',  
    name,  
    callback === null ? 'null' : typeof callback,  
  );  
  viewConfigCallbacks.set(name, callback);  
  return name;  
}

```

드디어 끝에 닿았다!

위 코드에서도 중요해보이는 부분만 찝어보자면,
invariant 는 불변성 에러 핸들링 으로 보이고, 실제로 github 에서 코드를 봐도 굉장히 단순하다 https://github.com/zertosh/invariant

그래서 결국 viewConfigCallbacks.set 코드가 register 함수에서 핵심 코드가 될것 같다!

```javascript
// packages/react-native/Libraries/Render/shims/ReactNativeViewConfigRegistry.js

const viewConfigCallbacks = new Map<string, ?() => ViewConfig>();

...
export function register() => {
	...
	viewConfigCallbacks.set(name, callback);
	return name;
}

...
```

view를 등록하는 viewConfigCallbacks 를 보면 단순히 Map 자료구조이다! 이제 여기서 생기는 의문은 

- 이렇게 메모리에 등록해 둔 건 언제 빼가지?
- Rendering은 언제 어떻게  해주지?
- Native UI 의 기능과 연결부는 어디에 있지?

```javascript

const viewConfigs = new Map<string, ViewConfig>();

/**  
 * Retrieves a config for the specified view. 
 * If this is the first time the view has been used, 
 * This configuration will be lazy-loaded from UIManager.  
 */
 /* 검색해서 가보기! */
export function get(name: string): ViewConfig {  
  let viewConfig = viewConfigs.get(name);  
  if (viewConfig == null) {  
    const callback = viewConfigCallbacks.get(name);  
    ...
    viewConfig = callback();  
    ...  
    processEventTypes(viewConfig);  
    viewConfigs.set(name, viewConfig);  
  
    // Clear the callback after the config is set so that  
    // we don't mask any errors during registration.    
    viewConfigCallbacks.set(name, null);  
  }  
  return viewConfig;  
}
```


당장 해결할 수 없지만 다음 실마리는 그 아래에 있다.
바로 아래 있는 get() 함수가 있는데, 첫 get() 할때, viewConfigCallbacks Map 에 저장된 View 의 정보를 빼가는 형태이다.

그리고 viewConfigs Map 이라는 또다른 Map 구조에 set() 하고 반환해준다.

정확히 왜 이렇게 해주는지는 알길이 없지만, 이 get() 함수를 따라가보기로 하자!


IDE 로 코드 검색을 ReactNativeViewConfigRegistry.get 로 해보면

![[Pasted image 20240922042723.png]]

나오는데, 대략적으로 Render 와 관련된 코드로 보인다. 
각각의 파일들을 찾아보면, 공통된 코드를 찾아볼 수 있다.

```javascript

// 5개 파일중 대표로!!
// packages/react-native/Libraries/Render/implementations/ReactNativeRenderer-prod.js

/* 타고 가보기! */
var ReactNativePrivateInterface = require("react-native/Libraries/ReactPrivate/ReactNativePrivateInterface"),

...
	
var getViewConfigForType =  
    ReactNativePrivateInterface.ReactNativeViewConfigRegistry.get,  
  nextReactTag = 3;

function completeWork(current, workInProgress, renderLanes) {
	...
	
	type = getViewConfigForType(type);  
	var updatePayload = diffProperties(  
	  null,  
	  emptyObject,  
	  newProps,  
	  type.validAttributes  
	);  
	ReactNativePrivateInterface.UIManager.createView(  
	  current,  
	  type.uiViewClassName,  
	  renderLanes,  
	  updatePayload  
	);

	...
}

```

10000줄이 넘는 코드기 때문에 모든 코드를 다 살펴볼 수는 없었다. 코드가 너무 많고 가독성이 좋지않아서 한번 컴파일이 된 코드인건가? 싶었다.

그 중 ReactNativeViewConfigRegistry.get 과 연관있으면서 가장 직접적인 코드는 위 코드였다.

```javascript
ReactNativePrivateInterface.UIManager.createView(  
  current,  
  type.uiViewClassName,  
  renderLanes,  
  updatePayload  
);
```

그 중 가장 중요해보이는 UIManager.createView() 였다. 결국 createView 에 대한 담당은 UIManager 라는 객체가 담당하는 걸로 보인다.











## 번외

RCTText 로 iOS, Android 를 각각 매칭해서 보여줄텐데, iOS는 바로 찾을수 있는데, 이상하게도 android 는 프로젝트 내에서 찾을 수 없었다.

```markdown
// packages/react-native/android/README.md

Starting from React Native 0.71, we're not shipping the `/android` folder inside the React Native NPM package anymore due to sizing constraints on NPM. The Android artifacts are distributed via Maven Central. You can read more about it in this RFC: [https://github.com/react-native-community/discussions-and-proposals/pull/508](https://github.com/react-native-community/discussions-and-proposals/pull/508)


```



android 
https://mvnrepository.com/artifact/com.facebook.react/react-android/0.76.0-rc.0