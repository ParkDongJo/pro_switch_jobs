
# iOS

```
yarn ios --simulator="XXXX"
```

했을 시

```
error Failed to launch the app on simulator, An error was encountered processing the command (domain=com.apple.CoreSimulator.SimError, code=405):
```

XCode 캐시로 인한 이슈

시스템 설정 > 일반 > 정보 > 저장공간 
- "저장 공간 설정" 클릭
- 개발자 > 
	- Xcode 캐시
	- 프로젝트 빌드 데이터 및 인덱스
	모두 삭제 후 재 빌드





# Android

gradle 셋팅 변경후

`Caused by: java.io.IOException: Cannot run program "node"`

아래 명령어 실행

```
sudo chmod +x /Applications/Android\ Studio.app/Contents/bin/printenv
```

https://flamingotiger.github.io/frontend/ReactNative/react-native-android-cannot-run-program-node/





리액트 네이티브 개발자들이 마주하는 이슈와 해결책 5가지
https://yozm.wishket.com/magazine/detail/1476/