
## 3-way merge
feature 브랜치가 main 브랜치로 병합 시 새로운 merge commit 이 main 에 추가되면서 병합을 한다.

feature 브랜치와 main 브랜치의 히스토리가 둘 다 보존이 된 채로 merge 가 된다.
![[Pasted image 20251124075453.png]]

장점
- 각 브랜치마다의 관계와 진행상황을 파악할 수 있음
- 문제가 되는 프로젝트(branch)단위 지점을 빠르게 revert 할 수 있음

단점
- 히스토리가 매우 복잡해짐
- 작업(commit)단위로 파악하기가 쉽지 않음

## fast forward merge
feature 브랜치가 main 브랜치로 병합 시 main 브랜치에 새로운 변경점이 없을 때, main 브랜치에 merge commit 을 생성하지 않고, main 브랜치의 HEAD를 feature 브랜치의 HEAD로 이동하여 병합을 한다.

쉽게 이야기 하면 feature 브랜치가 그때부터 main 브랜치가 된다

이런 형식이 싫으면, `git merge --no-ff` 라는 옵션으로 merge 를 하면 3-way-merge 로 된다.

![[Pasted image 20251124080322.png]]


장점
- main 브랜치의 히스토리가 간단하게 유지된다
- 자동으로 되니까. 신경쓸 필요 없다

단점
- 각 브랜치마다의 관계를 알기 힘들다. 

## squash (and) merge
feature 브랜치가 main 브랜치로 병합 시, feature 브랜치의 모든 브랜치가 하나의 commit 으로 합쳐져서 main 브랜치에 병합이 된다.
![[Pasted image 20251124082141.png]]

장점
- main 브랜치의 히스토리가 간단하게 유지된다
- merge 를 할 때, 충돌과 같은 불편함이 없다 (큰 신경을 안써도 된다)
- feature 브랜치의 각 작업(commit) 단위를 신경쓰지 않아도 된다 (작업이 편하다)

단점
- feature 브랜치의 작업(commit)단위를 파악하기 힘들다
-  각 브랜치마다의 관계를 알기 힘들다.



## rebase (and) merge
feature 브랜치가 main 브랜치로 병합 시, feature의 커밋을 main 브랜치 위에서 새로운 커밋 아이디로 재생성하여, 재배치 시키는 형태로 진행된다

![[Pasted image 20251124081209.png]]

장점
- main 브랜치의 히스토리가 한줄로 유지된다
- feature 브랜치의 작업(commit)단위를 파악하기 쉽다

단점
- main 브랜치와의 충돌이 있을 수 있으며, 이를 해결할 때 문제가 발생할 수도 있다
- feature 브랜치를 운영할 시 최신화를 습관적으로 해줄 필요가 있다
-  각 브랜치마다의 관계를 알기 힘들다.



#### 참고
https://codingapple.com/unit/git-rebase-squash/
https://dev-district.tistory.com/21
