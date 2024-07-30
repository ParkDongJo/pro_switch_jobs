
# 만약 수동으로 배포 한다면

- apk 파일 생성
	- ./gradlew build
	- ./gradlew assemble{Debug | Release}
		- {} mode 로 build {} APK 를 포함한 archive 생성
- 사이트에 접속
	- Google Play Console 사이트 접속
		- 난독화 처리
		- Production > Create New Release
		- 난독화 된 .apk 파일 업로드
	- Firebase Console 사이트 접속
		- App Distribution 선택
		- .apk 파일 업로드


# 앱 설치

Firebase 방법
- Firebase 에서 다운 받기
	- Firebase App Distribution 앱 다운로드 링크 접속
	- Firebase Profile 설치
	- 설정 > 일반 > VPN 및 기기 관리 > 구성 프로필 
- Apple Develop 사이트에 UUID 를 기반으로 기기 등록

Google Play Console 방법



# 앱 빌드 자동화

일반적인 순서

- 코드 받아오기
	- import_from_git
- 버전 업
	- increment_version
		- firebase_app_distribution_get_latest_release
		- increment_version_code
- 앱 빌드
	- gradle
	- build_android_app
- 앱 배포
	- firebase_app_distribution (firebase)
	- upload_to_play_store (play console)
- 앱 배포 후 알림
	- slack


# Firebase Distribution Plugin
[문서](https://firebase.google.com/docs/app-distribution/android/distribute-fastlane?hl=ko)




# 안드로이드 파일 유형
- APK
- AAB
- AAR



[Gradle Task 종료](https://velog.io/@alsgus92/Android-Gradle-Task%EB%8A%94-%EB%8F%84%EB%8C%80%EC%B2%B4-%EC%96%B4%EB%96%A4-%EC%97%AD%ED%95%A0%EC%9D%84-%EC%88%98%ED%96%89-%ED%95%98%EB%8A%94-%EA%B1%B8%EA%B9%8C#:~:text=gradle%20assembleRelease%20%EB%8A%94%20Android%20app,%EB%82%9C%EB%8F%85%ED%99%94%20%EB%B0%8F%20%EC%B5%9C%EC%A0%81%ED%99%94%EB%A5%BC)