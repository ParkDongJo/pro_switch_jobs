react-native 가 Core와 그 외 기능들로 분리되어 있다.

https://github.com/react-native-community/discussions-and-proposals/blob/main/proposals/0759-react-native-frameworks.md#summary

에 보면, react native frameworks 라고 타이틀을 달았는데, react-native Core 를 제외한 나머지의 큼직한 단위들을 framework 라고 표현하는 듯 하다. 그 중 가장 대표적인 것이 expo, react-native-comunity/cli 등등이 있다.

react-native core 는 주로
- 화면 렌더링
- 네이티브 모듈
- 네이티브 환경
- 빌드 시스템

등등에 주로 집중하고 있는 것으로 보인다.


그 외에 react-native frameworks 들은
- 프로젝트 템플릿
- 빌드 도구
- 카메라
- 알림 푸시
- Webview
- Firebase Analytics
- 네비게이션 라우팅

등등 앱에서 필요로 하는데, Core에서 제외된 주요 기능들을 다루고 있다.


react-native 는 Core를 가볍게 하여, 기여도를 쉽게 관리하기 위하여 기존의 비핵심(자칭) 컴포넌트들을 다 외부로 이전했는데, 그 와 관련된 비핵심 컴포넌트 목록이 이 페이지에 정리가 되어 있다.
https://github.com/facebook/react-native/issues/23313

이 frameworks 들은 각각의 대표적인 커뮤니티들이 이끌어 가고 있는데, 그중 가장 대표적인 커뮤니티가 firebase와 관련해서는 react-native-firebase 이다.

왜 이런 결정을 내리게 되었는지, 2018년 부터 시작된 논의와 이런 결정을 통해 얻게되는 이점들을 기재해뒀는데, 시간날 때 읽어보면 재미있다.





# Core에 집중하기
Core는 meta 에서 직접 관리하고 있다. 그렇다고 해서 아예 기여를 못하는건 아닌데, 코어 개발에 기여하는 방법을 기재해뒀다.

기여를 하든 안하든 https://reactnative.dev/contributing/overview 한번 읽어보는건 좋을것 같다

Core 는 packages/react-native/index.js 에서 부터 코드가 시작되는데, 이를 아래와 같은 순으로 코드를 읽고 정리해보면 좋을것 같다.


