

[TextView는 어떻게 렌더링 되는가](./react-native TextView) 에 이어서 코드 타고 가기를 이어가보자.

ReactNativePrivateInterface.UIManager 를 토대로 아래와 같이 찾아가보면,

-> react-native/Libraries/ReactPrivate/ReactNativePrivateInterface
-> react-native/Libraries/ReactNative/UIManager

```javascript
// packages/react-native/Libraries/ReactNative/UIManager.js

/* 타고 가보기! */
const UIManagerImpl: UIManagerJSInterface =  
  global.RN$Bridgeless === true  
    ? require('./BridgelessUIManager')  
    : require('./PaperUIManager');
    
const UIManager: UIManagerJSInterface = {  
  ...UIManagerImpl,  
  measure() {...},
  measureInWindow() {...},
  measureLayout() {...},
  measureLayoutRelativeToParent() {...},
  dispatchViewManagerCommand() {...},
};

module.exports = UIManager;


```

이제 조금씩 react-native 가 화면에 컴포넌트를 그리는 원리가 윤곽이 드러난다. 나중에 더 자세히 살펴봐야겠지만, 컴포넌트, 화면, 레이아웃, 부모와의 관계 등등을 측정하는 함수들이 여기에 다 들어가 있다.

- 정확히 rendering 이 무엇으로 인해 그려지는지
- 어떻게 그려지는지

이 과정을 더 살펴보려면, `FabricUIManager` 를 살펴봐야 할것 같다.

```javascript
import {getFabricUIManager} from './FabricUIManager';

...

const UIManager: UIManagerJSInterface = {  
  ...UIManagerImpl,  
  measure() {
	...
	const FabricUIManager = nullthrows(getFabricUIManager());
	...
  }
};

```

하지만 지금의 목적은 JavaScript 단과 Native UI 가 어떻게 이어지고, 통신하는지 이해하기 위한 과정이니까
아래의 코드를 타고가는게 좋을것 같다.

이유는 `UIManager.createView()` 가 이 코드에서는 찾아볼 수 없기 때문이다. 
그렇다면 UIManagerImpl 를 보면 아래와 같이

```javascript
const UIManagerImpl: UIManagerJSInterface =  
  global.RN$Bridgeless === true  
    ? require('./BridgelessUIManager')  
    : require('./PaperUIManager');

```

Bridge의 여부에 따라서 BridgelessUIManager, PaperUIManager 모듈을 뽑아내고 있다,
따라가보자


```javascript

// packages/react-native/Libraries/ReactNative/PaperUIManager.js

/* 타고 가보기! */
import NativeUIManager from './NativeUIManager';

...

const UIManagerJS: UIManagerJSInterface = {  
  ...NativeUIManager,  
  createView(  
    reactTag: number,  
    viewName: string,  
    rootTag: RootTag,  
    props: Object,  
  ): void {  
	...
  
    NativeUIManager.createView(reactTag, viewName, rootTag, props);  
  },
}

```


```javascript
// packages/react-native/Libraries/ReactNative/NativeUIManager.js

export * from '../../rc/private/specs/modules/NativeUIManager';  

/* 타고 가보기! */
import NativeUIManager from '../../src/private/specs/modules/NativeUIManager';  
export default NativeUIManager;

```


```javascript
// packages/react-native/src/private/specs/modules/NativeUIManager.js

/* 타고 가보기! */
import * as TurboModuleRegistry from '../../../../Libraries/TurboModule/TurboModuleRegistry';

export interface Spec extends TurboModule {
	...
}
export default (TurboModuleRegistry.getEnforcing<Spec>('UIManager'): Spec);

```


```javascript
// pacakges/react-native/Libraries/TurboModule/TurboModuleRegistry.js

/* 타고 가보기! */
const NativeModules = require('../BatchedBridge/NativeModules');

const turboModuleProxy = global.__turboModuleProxy;


function requireModule<T: TurboModule>(name: string): ?T {  
  if (turboModuleProxy != null) {  
    const module: ?T = turboModuleProxy(name);  
    if (module != null) {  
      return module;  
    }  
  }  
  
  if (useLegacyNativeModuleInterop) {  
    // Backward compatibility layer during migration.  
    const legacyModule: ?T = NativeModules[name];  
    if (legacyModule != null) {  
      return legacyModule;  
    }  
  }  
  
  return null;  
}

export function getEnforcing<T: TurboModule>(name: string): T {  
  const module = requireModule<T>(name);  
  ... 
  return module;  
}
```



```javascript
// pacakges/react-native/Libraries/BatchedBridge/NativeModules.js

function genModule(  
  config: ?ModuleConfig,  
  moduleID: number,  
): ?{  
  name: string,  
  module?: {...},  
  ...  
} {  
  if (!config) {  
    return null;  
  }  
  
  const [moduleName, constants, methods, promiseMethods, syncMethods] = config;  
  invariant(  
    !moduleName.startsWith('RCT') && !moduleName.startsWith('RK'),  
    "Module name prefixes should've been stripped by the native side " +  
      "but wasn't for " +  
      moduleName,  
  );  
  
  if (!constants && !methods) {  
    // Module contents will be filled in lazily later  
    return {name: moduleName};  
  }  
  
  const module: {[string]: mixed} = {};  
  methods &&  
    methods.forEach((methodName, methodID) => {  
      const isPromise =  
        (promiseMethods && arrayContains(promiseMethods, methodID)) || false;  
      const isSync =  
        (syncMethods && arrayContains(syncMethods, methodID)) || false;  
      invariant(  
        !isPromise || !isSync,  
        'Cannot have a method that is both async and a sync hook',  
      );  
      const methodType = isPromise ? 'promise' : isSync ? 'sync' : 'async';  
      module[methodName] = genMethod(moduleID, methodID, methodType);  
    });  
  
  Object.assign(module, constants);  
  
  if (module.getConstants == null) {  
    module.getConstants = () => constants || Object.freeze({});  
  } else {  
    console.warn(  
      `Unable to define method 'getConstants()' on NativeModule '${moduleName}'. NativeModule '${moduleName}' already has a constant or method called 'getConstants'. Please remove it.`,  
    );  
  }  
  
  if (__DEV__) {  
    BatchedBridge.createDebugLookup(moduleID, moduleName, methods);  
  }  
  
  return {name: moduleName, module};  
}

```

genModule() 코드를 보면 단순히 메모리에 있는 module을 가져오는 코드다. 

turboModuleProxy 을 봐야한다



## Native UIManager 의 위치

Android -> 
packages/react-native/ReactAndroid/src/main/java/com/facebook/react/uimanager/UIManagerModule.java

iOS ->
packages/react-native/React/Base/RCTRootView.m