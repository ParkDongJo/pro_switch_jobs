
## React 에서 Lottie
Lottie 란 에어비앤비에서 개발한 JSON 기반의 애니메이션 라이브러리

LottieFiles에서 원하는 lottie를 찾아 json으로 다운로드하여 적용
https://lottiefiles.com/kr/what-is-lottie



## React Native 에서 Lottie
https://github.com/lottie-react-native/lottie-react-native




라이브러리 더 파기
## lottie-react-native
Lottie 는 JSON 파일로 애니메이션 정의를 통해, 구현을 하는 형태이다.

web 뿐만 아니라 iOS, AOS 등등 airbnb 에서 제공하는 라이브러리가 있어보인다.
https://airbnb.io/lottie/#/

거기에 ReactNative 도 포함이 되어 있는데, 결국 iOS 나 Android 의 sdk 를 import 해서 동작하게끔 되어 있는 것 같다. (물론 좀 더 자세히 보고 정리해볼 필요가 있다.)
https://github.com/lottie-react-native/lottie-react-native/blob/master/packages/core/src/specs/LottieAnimationViewNativeComponent.ts

## 사용법
Lottie 의 사용방법은 아래 github 레포의 코드를 참고하는 것이 좋아 보인다.
https://github.com/aaronbesson/react-native-lottie-rating/tree/master

개인이 lottie-react-native 를 활용해서 별평점 애니메이션을 구현한 코드이다.
코드는 정말 특별할 것이 없고,

- animation.json (https://github.com/aaronbesson/react-native-lottie-rating/blob/master/animation.json)
- react-native
	- TouchableOpacity 사용
- lottie-react-native 

가 전부 이다. [코드링크](https://github.com/aaronbesson/react-native-lottie-rating/blob/master/rating.js)

README.md에 링크가 걸려있는데, 애니메이션 json 은 아래 링크의 파일을 활용한 것으로 보인다.
https://github.com/aaronbesson/react-native-lottie-rating/blob/master/rating.js

lottieFiles 라는 곳으로 보이는데, 여기서 마음에 드는 애니메이션 json 파일을 다운 받아서 셋팅하는 구조로 보인다.


## Lottie 파일 만드는 법
Lottie JSON 파일을 만드는 법은 아래 lottieFiles 에 영상으로 올라가있다. 다음에 여유될 때 살펴보면 되겠지만 어도비를 사용해야하는 걸 보면, 여기서 부터는 디자인의 영역이다.

개발자인 나는 취미삼아 해본다거나, 또는 이미 나와있는 Lottie 애니메이션을 잘 활용하는 것이 효율적으로 보인다.
https://lottiefiles.com/tutorials/how-to-make-a-simple-lottie-animation-json-with-after-effects-and-bodymovin-AGsOY4id6CA




```
Logging event:

origin=app,
name=click,
params=Bundle[
  {
    screen_name=home, 
    ga_event_origin(_o)=app, 
    ga_screen_class(_sc)=MainActivity, 
    ga_screen_id(_si)=8821527009210762008, 
    ga_screen(_sn)=home, 
    adid=9dca650d10690db6, 
    category=home, 
    event_name=header_favorite
    }
]

```