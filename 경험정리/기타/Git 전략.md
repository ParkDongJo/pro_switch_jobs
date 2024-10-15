기존 브랜치 관리가 제대로 되어있지 않아서, 이번에 git flow 브랜치 전략을 도입하게 되었다. 물론 약간의 변형은 있겠지만 

- main
- develop
	- int
	- feature
- release
- hotfix

등등 의 브랜치들의 각각의 역할과 관계를 정의하고 이를 정책화 하였다. 이번에 [빌드](obsidian://open?vault=pro_switch_jobs&file=%EA%B2%BD%ED%97%98%EC%A0%95%EB%A6%AC%2F%EB%B9%8C%EB%93%9C) 관련된 이슈에서도 잠깐 언급했지만 우리는 gitlab ci 를 활용하고 있다.

gitlab-ci 를 통해서 특정 브랜치가 돌았을 시, 정의된 script 를 실행해줄 수 있다. 이를 활용해서 아래와 같은 작업을 실행하도록 파일을 작성해뒀다.

- 기존 develop, int 브랜치 삭제 후 main 의 HEAD 기준으로 재생성

그리고 아래와 같은 작업을 추가로 gitlab 에 해줬는데

- 주요 브랜치들에 대해 push, force push, 코드 승인 등등을 제한했음
	- Settings > Repository > Protected branches
- 주요 환경변수 gitlab 에 등록
	- Settings > CI/CD > Variables
- 브랜치 이름 패턴 제한함 (feature/hotfix/release 등등)
	- Settings > Repository > Push rules
- 기본 브랜치
	- Settings > Repository > Branch defaults
- 개발자 역할 재정의 (maintainer 는 은석님만 할당 그 외에 developers)
	- Project information > Members

특히 주요 브랜치들에 대해 push, force push 등등 을 제한하기 위해서는 개발자들의 역할 정의부터 해야한다.
역할에 대한 개념은 아래와 같이 정의 되어 있다.

```
- **Guest (private and internal projects only)**
    - 이슈를 만들거나 댓글 작성 가능 → 제한적 협업
        
- **Reporter**
    - Git Repository의 파일의 버전을 변경할 수 없음
    - Group Epic 생성 가능
    - 이슈 담당자 지정 등의 제한된 이슈 관리 활동과 Merge Request 생성 가능
        
- **Developer (개발자)**
    - GitLab 프로젝트 및 그룹에 포함된 에픽이나 이슈들을 생성, 수정 및 보완 할 수 있음
    - 제약이 설정된 Branch에 Push할 수 없지만, 별도의 Branch를 생성하여 Git Repository의 파일을 변경
    - 변경이 완료되면 Merge Request를 생성하여 승인을 요청
        
- **Maintainer (PL/PM)**
    - Master Branch에 Push 할 수 있고, Merge Request를 승인할 수 있음
    - Group/Project/구성원 괸리를 제외하고 Owner의 대부분 권한이 포함됨
        
- **Owner (PM/관리자)**
    - Group 또는 Project를 생성한 사람 → Group과 Project의 모든 권한을 가지고 있음
    - Member 추가 및 초대 가능 → Member의 역할 부여
```


현재는 우리 팀원들은 모두 Maintainer로 정의 되어 있다. 브랜치 push, create, MR 등등을 제한하려면 Maintainer 이상은 리더급 또는 CI만 가지고, 그 외 개발자들은 developer 이하가 되어야 한다.


## 개선이 더 필요한 점
release/* 브랜치 같은 경우! main 브랜치가 배포 된 이후 develop (feature 를 따는 기준 브랜치)를 다시 생성하고 develop 기준으로 release/* 브랜치를 따도록 했다.

이때, release/* 가
- 자동 생성 되고
- default 브랜치로 설정되면
좋은데, 이 부분들을 자동으로 설정이 된다면 매우 편해진다. 우선은 아래와 같은 Script 로 나눠볼 수 있다.


```
# 다음 버전을 자동으로 생성  
VERSION=$(  
  curl --header "PRIVATE-TOKEN: $PRIVATE_TOKEN" "$GITLAB_API_URL/projects/$CI_PROJECT_ID/repository/tags" |  
    jq -r '.[0].name' |  
    sed 's/selection.v//g' |  
    awk -F. '{print $1"."$2+1"."$3}'  
)  
echo "Create release/v$VERSION branch"  
  
```


```
# 생성된 release 브랜치를 default branch 로 지정
curl --request PUT --header "PRIVATE-TOKEN: $PRIVATE_TOKEN" "$GITLAB_API_URL/projects/$CI_PROJECT_ID" --form "default_branch=release/v$VERSION"
```


물론 여전히 고민해야 할 부분은 있다. 버전업 같은 경우 `MAJOR.MINOR.PATCH` 에서 그 때의 상황에 따라 다르게 적용될 수 있는데 이 부분은 어떻게 해결하냐 이다.

최소한의 입력값은 받아야 할것 같은데, 이 부분에 대한 보완이 필요하다.
