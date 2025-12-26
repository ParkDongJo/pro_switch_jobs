

> 위의 렌더 트리에서 각 컴포넌트가 렌더링하는 HTML 태그에 대한 언급이 없음을 알 수 있습니다. 이는 렌더 트리가 React [컴포넌트](https://ko-react-exy5xcwjj-fbopensource.vercel.app/learn/your-first-component#components-ui-building-blocks)로만 구성되어 있기 때문입니다.
>UI 프레임워크로서 React는 플랫폼에 독립적입니다. react.dev에서는 HTML 마크업을 UI 기본 요소로 사용하는 웹을 렌더링하는 예시를 보여줍니다. 하지만 React 앱은 모바일이나 데스크톱 플랫폼에 렌더링 될 수 있으며, 이러한 플랫폼은 [UIView](https://developer.apple.com/documentation/uikit/uiview)나 [FrameworkElement](https://learn.microsoft.com/en-us/dotnet/api/system.windows.frameworkelement?view=windowsdesktop-7.0)와 같은 다른 UI 기본 요소를 사용할 수 있습니다.
>
이러한 플랫폼 UI 기본 요소는 React의 일부가 아닙니다. React 렌더 트리는 앱이 렌더링되는 플랫폼에 관계없이 React 앱에 대한 통찰력을 제공할 수 있습니다.


#### 의문
UIView 에 바로 React 의 렌더 트리가 매칭될 수 있는가? UIView 라고 하면 그 UI 의 정보에 맞게 재설정이 되어야 하지 않을까? 그것에 대한 예시가 ReactNative 일것 같은데, 이 부분에 대해서는 좀 더 알아볼 필요가 있음

아! 그렇다면? React Native 안에 React 를 사용하고 있는데, 결국 기본적인 UI 에 대한 것은 그 플랫폼에 맞게 정보를 읽어 오고 ~ 구성을 한다는 것??

그럼 React Native 는 앱에서만 특화된 기능을 수행하기 위한 Wrapper 형태의 모바일 프레임워크라고 봐야할까?
