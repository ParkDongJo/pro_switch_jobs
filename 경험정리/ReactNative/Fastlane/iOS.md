
# 만약 수동으로 배포 한다면

- .ipa (iOS Store Package) 파일 생성
	- Xcode 에서 [Any iOS Device] 타겟으로 빌드한다
	- Xcode > Product > Archive
	- Xcode > Window > Organizer > [Distribute App] 선택
		-  빌드 in Xcode
			- AppStore 선택 : AppStore 업로드
			- Upload 선택 -> Next 클릭
			- 옵션 선택 -> Next 클릭
			- Certificate & Profile 선택
			- Upload 선택
		- 빌드 in Xcode
			- Development 선택 : .ipa 파일 생성
			- 기본 옵션 선택 -> Next 클릭
			- Signing 방식 선택 -> Automatically manage signing
			- Export 선택 -> 생성할 폴더 선택
- 사이트에 접속
	- Apple Develop 사이트 접속
		- TestFlight 선택
		- 테스트 세부사항 / 테스터 추가
		- [수동으로 버전 출시] 서택
	- Firebase Console 사이트 접속
		- App Distribution 선택
		- .ipa 파일 업로드


# 앱 설치

Firebase 방법
- Firebase 에서 다운 받기
	- Firebase App Distribution 앱 다운로드 링크 접속
	- Firebase Profile 설치
	- 설정 > 일반 > VPN 및 기기 관리 > 구성 프로필 
- Apple Develop 사이트에 UUID 를 기반으로 기기 등록

TestFlight 방법
- TestFlight 에서 다운 받기
	- TestFlight 앱 설치
	- 로그인 후 앱 설치



# 앱 빌드 자동화

일반적인 순서

- 코드 받아오기
	- import_from_git
- 인증
	- cert
	- sign
- 버전 업
	- increment_build_number
- 앱 빌드
	- build_app
- 앱 배포
	- firebase_app_distribution (firebase)
	- upload_to_testflight (store)
- 앱 배포 후 알림
	- slack

인증을 위한 기본 task 에서는 아래 api 들이 있어 보인다.
(https://docs.fastlane.tools/app-store-connect-api/)
![[Pasted image 20240729164635.png]]