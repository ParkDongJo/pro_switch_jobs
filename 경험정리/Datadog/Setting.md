ReactNative 에 Datadog 을 설정하기 위해서는 몇가지 설정을 해야한다.

- 사이트 설정
- 코드단 설정
	- jsx 단 설정
	- config 단 설정
	- Native 단 설정
		- iOS Build Phaces
		- Android gralde

여기서 대부분의 설정은 [데이터독 doc](https://docs.datadoghq.com/ko/real_user_monitoring/mobile_and_tv_monitoring/setup/reactnative/) 에 설명이 되어 있기 때문에, 이에 대해서는 좀 더 정제된 내용으로만 가볍게 나열하고자 한다.
그 외에 중요한 것은 좀 더 디테일한 사항들과 마주 할 수 있는 것들인데 그런 내용을 정리해놓고자 한다


# 사이트 셋팅!!
사이트 설정은 Application 등록을 하고 Client Token 을 발급 받아야 한다.


# 코드 셋팅!!
문서가 잘 되어있는 듯 하면서도 내용이 혼재되어 있다.  그래서 작업의 순서를 먼저 간략히 나열해보자

- 라이브러리 설치
	- datadog/mobile-react-native
- config 코드 셋팅
- DatadogProvider 에 config 주입
- DdRumReactNavigationTracking 의 startTrackingViews, stopTrackingViews 셋팅

여기까지가 데이터독 문서에 나와있는 내용이다. 

# 코드 추가 셋팅!!
코드 단에서 좀 더 신경써야 할 부분이 있다. 내가 우선 셋팅한 내용은 아래와 같다

- react-navigation
- 빌드 시 파일 업로드
	- sourcemap
		- android
		- ios


### react-navigation
navigation 외부 프레임워크는 RN에서 필수적으로 선택되는 프레임워크이다. 이를 사용할때는 아래와 같은 순서로 셋팅해야한다.

- 라이브러리 설치
	- datadog/mobile-react-navigation
- startTrackingViews, stopTrackingViews 셋팅

특별할 것은 없지만 trackngviews 를 NavigationContainer 의 onReady 에 기재해줘야 한다. 예제 코드를 가져오자면 아래와 같다

그리고 doc 에는 딱히 나와 있지 않지만, stopTrackingViews 는 View 가 unmount 되었을 시 셋팅을 해주는 것이 좋다
	
```jsx
import * as React from 'react';
import { DdRum, ViewNamePredicate } from '@datadog/mobile-react-navigation';
import { Route } from "@react-navigation/native";

const viewNamePredicate: ViewNamePredicate = function predicate(route: Route<string, any | undefined>, trackedName: string) {
  return "My custom View Name"
}

function App() {
  const navigationRef = React.useRef(null);

	useEffect(  
	  () => () => {  DdRum.stopTrackingViews(navigationRef?.current);  
	  },  
	  [],  
	);

  return (
    <View>
      <NavigationContainer ref={navigationRef} onReady={() => {
        DdRum.startTrackingViews(navigationRef.current, viewNamePredicate)
      }}>
        // …
      </NavigationContainer>
    </View>
  );
}
```


### 빌드 시 파일 업로드
기본 문서에는 없지만 좀 더 진행을 하다 보면, release 배포를 하게 되는 경우 봐야할 내용이 있다. 바로 JavaScript 단의 sourcemap 과 Native 단의 dSym 과 progaurd  내용이다. 

우선 여기서는 JavaScript 의 sourcemap 업로드 내용 부터 알아보자.

이때 먼저 빌드 자동화를 위한 셋팅이 필요하다. [깃 문서](https://github.com/DataDog/datadog-ci/tree/master/src/commands/react-native#xcode)

- datadog-ci 설치
- datadog-ci.json 에 apiKey 설정

빌드에 대한 설정 자체는 매우 간단하다. 그리고 문서에는 수동으로 bundle 파일과 bundle.map 파일을 업로드 하는 방법이 기재되어 있다.

하지만, 누구나 자동화를 원한다. 그러기 위해서는 각 OS 마다 셋팅을 해줘야 한다. 문서는 [충돌보고](https://docs.datadoghq.com/ko/real_user_monitoring/error_tracking/mobile/reactnative/) 에 기재되어 있다.

먼저 확인해야할 설정 옵션들이 있다.


#### 옵션 확인

- javascript 충돌 보고 옵션
- native 충돌 보고 옵션

```javascript
const config = new DdSdkReactNativeConfiguration(
    '<CLIENT_TOKEN>',
    '<ENVIRONMENT_NAME>',
    '<APPLICATION_ID>',
    true,
    true,
    true // enable JavaScript crash reporting
);
config.nativeCrashReportEnabled = true; // enable native crash reporting
```


# 빌드 시 파일 업로드 > Sourcemap

#### API KEY 발급
datadog 은 sourcemap 같은 파일을 업로드 하기 위해서는, 자체 서버의 API_KEY 가 필요로 하다. 자칫 문서를 보고, Client Token 를 API Key 로 오해하는 경우가 있다.

하지만 둘은 다르다. 자세한 문서는 [API 키](https://docs.datadoghq.com/ko/account_management/api-app-keys/#api-keys) 에 있지만, 간단하게 정리해보자면 아래와 같다

- API KEY - 이벤트를 Datadog 으로 전송하려면 이 API KEY 가 필요하며, sourcemap 같은 파일을 업로드 할 시에도 필요하다.
	- Site 에서 API Keys 에 목록으로 나온다.
	- Site 에서 API Keys 에서 새 key 발급이 가능하다. (권한이 없다면, 인프라 팀에 요청)
	- datadog-ci.json 에 설정을 한다.
- Client Token - API Key 는 보안상의 이유로 웹브라우저, Android, iOS 등등의 클라이언트에서는 사용할 수가 없다. 이때 public 용으로 client 에서 자신을 증명하기 위해 사용되는 Token 이다.
	- Site 에서 Digital Experience > Add an Application 에서 서비스를 등록하면 자동으로 발급된다.
	- 코드단 config 에 설정을 하면 된다.

#### [Native 설정](./native_config)
android 경우 app/build.gradle 상단에 아래의 설정을 셋팅해주면 된다. 단 주의 할 점은 com.facebook.react 보다는 아래 있어야 한다.

```gradle
apply from: "../../node_modules/@datadog/mobile-react-native/datadog-sourcemaps.gradle"
```

iOS 경우 Xcode 의 `Targets > Build Phases > Bundle React Native code and images` 에서 shell script 코드를 통해서 빌드 전 js 빌드를 위해 실행하는 코드를 작성해줘야 한다. 아래에서 더 자세히 다룰 거지만 아래와 같이 해줘야 한다.

```shell
#!/bin/sh
export NODEBINARY=node
DATADOG_XCODE="../node_modules/.bin/datadog-ci react-native xcode"

/bin/sh -c "$DATADOG_XCODE"
```

이렇게 하면 실제로 shell script 로 인해 찍히는 실행되는 명령어는 아래와 같이 완성된다.

```shell
/usr/local/bin/node ../node_modules/.bin/datadog-ci react-native xcode
```


#### ReactNative 버전별 설정

##### Android
Android 같은 경우 위에 [Native 설정](./native_config) 에 설명해놓은데로 하면 된다. 다만 ReactNative 가 한창 버전별 업그레이드가 되고 있는 중이라 버전별로 설정이 나뉜다.

Android 에서는
- React Native >= 0.71
- React Native < 0.71
- 그 외 각 빌드 수동으로 할 시

여기서는 무조건 최신버전 기준으로 정리하고자 한다.

__android/app/build.gradle__
```
apply from: "../../node_modules/@datadog/mobile-react-native/datadog-sourcemaps.gradle"
```

__datadog-ci.json__
```json
{
    "apiKey": "<YOUR_DATADOG_API_KEY>"
}
```


##### iOS
iOS 설정에서 좀 애먹었는데, 일단 문서에서는 부족한 내용이 있다. 각 프로젝트 환경의 차이인지는 모르겠지만, 실제로 github 의 내용과 공식 홈페이지 문서 내용도 조금씩 다르다. 우선은 github 문서를 기준으로 하는 것이  맞다. [데이터독ci](https://github.com/DataDog/datadog-ci/tree/master/src/commands/react-native#xcode)


iOS는
- React Native >= 0.69
- React Native < 0.69
-  그 외 각 빌드 수동으로 할 시

각각 메뉴얼이 있고 마찬가지로 최신버전으로 정리해보자.

먼저 아래 sh 파일을 ios 에 생성해둔다

__ios/data-sourcemap.sh__
```shell
#!/bin/sh  
  
REACT_NATIVE_XCODE="../node_modules/react-native/scripts/react-native-xcode.sh"  
DATADOG_XCODE="../node_modules/.bin/datadog-ci react-native xcode"  
  
/bin/sh -c "$DATADOG_XCODE $REACT_NATIVE_XCODE"
```

__Targets > Build Phases > Bundle React Native code and images__
```shell
set -x -e

SOURCEMAP_FILE="$DERIVED_FILE_DIR/main.jsbundle.map"

WITH_ENVIRONMENT="../node_modules/react-native/scripts/xcode/with-environment.sh"

REACT_NATIVE_XCODE="./datadog-sourcemaps.sh"
  

/bin/sh -c "$WITH_ENVIRONMENT $REACT_NATIVE_XCODE"
```

이 모든 과정은 node 에 의해서 실행되어진다. 그래서 내부적으로 NODE_BINARY 변수에 node 실행 파일 위치를 넣어준다.

문제는 node 의 위치가 각 환경 마다 다를 수 있다는 것이다.
예를 들어 나같은 경우 nvm 을 사용하는데 nvm 의 node 위치는 다르다.

이때 Xcode 에서는 .xcode.env 에 환경변수를 설정할 수가 있다. 일단 빌드 환경은 nvm 이 아니니 로컬에서만 사용하고자 .xcode.env.local 파일에 NODE_BINARY 를 셋팅해줬다.

__.xcode.env.local
```shell
# To use nvm with brew, uncomment the line below  
# . "$(brew --prefix nvm)/nvm.sh" --no-use  
export NVM_DIR="$HOME/.nvm"  
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  
export NODE_BINARY=$(command -v node)
```

이렇게 하면 기본적으로 node 의 위치를 nvm path 로 잡아준다.

우리가 shell 의 파일을 좀 더 볼 필요가 있다.

- react-native > scripts > react-native-xcode.sh
- .bin > datadog-ci.sh
- react-native > scripts > xcode > with-environment.sh


__react-native-xcode.sh__
react-native-xcode.sh를 들여다보면 결국 이 파일은 node 를 통해서 

- main.jsbundle
- main.jsbundle.map

등등의 번들 파일과 그 외 번들링에 필요한 파일들을 생성해내는 것이 핵심이다. map 파일 같은 경우는 `--sourcemap-output` 이라는 키워드를 통해서 생성을 해주는데, 내부적으로 아래 두 변수 값에 의해 달리 생성된다. 

- $USE_HERMES
- $SOURCEMAP_FILE

USE_HERMES 같은 경우 디폴트 값이 true 이기 때문에, 값을 주지 않으면 true 의 로직을 탄다.

$SOURCEMAP_FILE 같은 경우는 DeriveDatas 의 경로에 bundle 파일과 함께 생성되도록 설정해주는 것이 좋다.

다른 경로를 설정해줘봤지만, 파일을 생성하고 copy하는 과정들이 있는데 결정적으로 업로드 시 sourcemap 을 찾지 못하는 오류가 계속 발생했었다.


__datadog-ci.sh__
react-native-xcode.sh 통해서 생성된 main.jsbundle과 main.jsbundle.map 파일을 datadog-ci 로 업로드 하는 역할을 한다.

내부 코드를 보면 arguments 로 들어온 키워드를 실행파일 path 로 삼고, 실행 파일을 하나씩 실행시키는 걸로 보인다.

```javascript
for (const commandFolder of fs_1.default.readdirSync(commandsPath)) {
	...

	require(`${commandPath}/cli`).forEach((command) => cli.register(command));

	...
}
```

__ios/data-sourcemap.sh__ 에 설정해둔 코드를 보면 실행파일은 `react-naitve`와 `xcode`이다


__with-environment.sh__
`Targets > Build Phases > Bundle React Native code and images` 에 with-environment.sh 가 함께 실행되는데, 사실 이 실행 파일은 별 하는 일은 없다.

내부적으로 코드를 보면 BINARY_NODE 를 셋팅해주고, 만약 node 의 위치를 찾지 못하면 내부적으로 `react-naitve/scripts/find-node-for-xcode.sh` 를 실행시켜주는데 node 의 위치를 찾아주는 역할을 한다. 하지만 이 파일은 추후에 deprecated 될 운명이라고 한다.


###### iOS 에서의 주의사항!!!!
iOS 에서 계속해서 빌드가 안되는 상황이 발생했다. 이는 또

Project > Build
Project > Archive

가 각각 다르다. Build 에서 성공했다 하더라도, Archive 에서는 실패할 수 있다. 이때 shell 스트립트에 2가지 옵션을 걸고 실행하는 것이 오류를 찾아가는데 도움이 된다.
```shell
set -x -e
```


> • **의미**: -x 옵션은 **명령어 추적**을 활성화합니다.
> 
   • **동작**: 스크립트가 실행되는 동안, 각 명령어가 실행되기 전에 해당 명령어를 **터미널에 출력**합니다. 즉, 실행되 고 있는 명령어와 그 인자들을 출력하여 디버깅을 쉽게 할 수 있게 합니다.

> • **의미**: -e 옵션은 **오류 발생 시 스크립트를 종료**하도록 설정합니다.
>
   • **동작**: 스크립트 내에서 실행된 명령어가 **오류를 반환하는 즉시 스크립트 실행을 중단**합니다. 여기서 “오류”는 명령어의 종료 코드가 0이 아닌 경우를 말합니다.


위 옵션을 켜고 오류를 추적해가면서 이리저리 테스트나 시도를 해보고, 오류를 고쳐갔다. 대부분 오류와 코드를 추적하다보면 어느정도 추측이 되지만 단, 한가지 추측이 힘든 경우가 있었다.

main.jsbundle.map 의 경로를 찾지 못하겠다는 오류 메시지가 계속 떴었다. 
둘 중 하나이다. 

- main.jsbundle.map 를 찾지 못했거나
- main.jsbundle.map 를 생성하지 않았거나

몇가지 확인을 계속 해봤지만

- map 파일의 생성 경로
- node 실행 파일 경로
- react-native-xcode.sh 에서 map 파일 생성에 영향을 주는 변수 확인
	- hermes
	- sourcefile_path

모두 해결방법이 되지 않았다.

그러던 중 로그에서 아래와 같은 로그를 보게 되었다.

![[Pasted image 20240929162414.png]]

bundle 파일과 bundle.map 파일을 실제 설정해둔 path 로 copy 를 하는 과정으로 보인다. 실제 bundle 파일은 올바른 경로로 복사되고 있지만
bundle.map 은 프로젝트에 Scheme Stage 가 빠진 채로 경로가 잡혀있었다.

이게 설마설마 하던 부분이지만,

`projectname Stage`, `projectname Int` 이런식의 환경 이름은 띄어쓰기를 했을 때, 공백 관련 문제가 스크립트 실행 시 문제가 될 수 있다.

실제로 공백을 제거해서 빌드 스크립트를 올렸더니, 빌드는 문제없이 잘 되었고 최종적으로 ios sourcemap 도 datadog 에 잘 업로드 되었다.

![[스크린샷 2024-09-29 오후 4.33.48.png]]




# 빌드 시 파일 업로드 > Dsym

https://docs.datadoghq.com/ko/real_user_monitoring/error_tracking/mobile/ios/?tab=cocoapods#symbolicate-crash-reports


# 빌드 시 파일 업로드 > Proguard

https://reactnative.dev/docs/signed-apk-android#enabling-proguard-to-reduce-the-size-of-the-apk-optional