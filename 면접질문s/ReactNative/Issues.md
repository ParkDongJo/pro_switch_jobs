
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
