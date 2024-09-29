

`git@github.com-pdj:{user_name}/{repo_name}.git` 형태의 원격 URL은, SSH 설정에서 `~/.ssh/config` 파일을 통해 별칭(alias)을 설정한 결과일 가능성이 매우 높습니다. `-pdj` 부분은 사용자가 설정한 별칭으로 보이며, 이 설정을 통해 여러 개의 SSH 키나 계정을 사용할 수 있습니다.

아래는 `~/.ssh/config` 파일에서 어떻게 별칭을 설정할 수 있는지에 대한 예시입니다:

### 1. **`~/.ssh/config` 파일 확인**
   먼저, `~/.ssh/config` 파일을 확인해보세요. 해당 파일이 없는 경우 직접 생성하면 됩니다. 이 파일은 SSH 연결에 대한 설정을 저장할 수 있는 곳입니다.

   예를 들어, `~/.ssh/config` 파일에 다음과 같은 내용이 있을 수 있습니다:

   ```bash
   Host github.com-pdj
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_rsa_pdj
   ```

   여기서:
   - `Host github.com-pdj`는 `-pdj`를 포함한 별칭(alias)입니다. `git remote` 명령어에서 `git@github.com-pdj` 형태로 사용됩니다.
   - `HostName github.com`은 실제로 접속할 호스트입니다.
   - `User git`은 SSH로 접속할 때 사용할 사용자 이름입니다.
   - `IdentityFile ~/.ssh/id_rsa_pdj`는 해당 호스트에 접속할 때 사용할 SSH 키 파일입니다. 즉, `~/.ssh/id_rsa_pdj`라는 특정 SSH 키를 사용하도록 설정된 것입니다.

### 2. **여러 개의 SSH 키 설정**
   이 설정을 통해, 여러 개의 SSH 키를 사용하는 경우 각 프로젝트마다 다른 SSH 키를 지정할 수 있습니다. 만약 다른 계정이나 특정 프로젝트마다 별도의 SSH 키를 사용해야 한다면, 이와 같은 방식으로 설정합니다.

### 3. **`git@github.com` 주소와의 차이점**
   `git@github.com:{user_name}/{repo_name}.git`는 GitHub에서 기본적으로 사용하는 URL 형식입니다. 위의 `-pdj`는 별칭을 통해 특정 SSH 키를 사용하도록 설정한 것일 뿐, 실제로 GitHub의 URL에는 영향을 미치지 않습니다. 본질적으로 둘 다 동일한 GitHub 저장소를 가리키지만, SSH 설정에 따라 다른 인증 방법(SSH 키)을 사용할 수 있게끔 만든 것입니다.

### 결론:
   `-pdj`는 SSH 설정 파일에서 별칭으로 설정된 것입니다. 이 별칭은 SSH 접속 시 어떤 SSH 키를 사용할지, 혹은 어떤 계정을 사용할지를 지정하는 데 유용합니다. `~/.ssh/config` 파일을 열어 설정 내용을 확인하고 필요한 경우 수정하거나, 추가적인 SSH 키를 등록할 수 있습니다.




로컬에서 B 레포의 `remote` 호스트 이름을 `github.com-pdj` 형태로 설정하려면, 다음과 같이 `~/.ssh/config` 파일에 `Host github.com-pdj`를 추가하고, B 레포의 원격 URL을 변경하면 됩니다.

### 1. **`~/.ssh/config` 파일 수정**

먼저 `~/.ssh/config` 파일을 열고, `github.com-pdj`에 대한 설정을 추가합니다. 이미 A 레포에서 사용하고 있는 설정과 동일하게 설정하면 됩니다.

```bash
Host github.com-pdj
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_pdj  # 사용할 SSH 키 경로
```

이 설정은 `github.com-pdj`라는 별칭을 사용해서 GitHub에 접속할 때 `~/.ssh/id_rsa_pdj`라는 특정 SSH 키를 사용하도록 지정한 것입니다.

### 2. **B 레포의 원격 URL 변경**

이제 B 레포의 `remote` URL을 `github.com-pdj` 형태로 변경해 줍니다. 해당 레포 디렉토리로 이동하여 다음 명령어를 실행하세요.

```bash
cd /path/to/B-repo
git remote set-url origin git@github.com-pdj:{user_name}/{repo_name}.git
```

위 명령어를 통해 B 레포의 원격 URL이 `github.com-pdj`로 변경됩니다.

### 3. **변경된 remote URL 확인**

변경이 제대로 되었는지 확인하려면 다음 명령어로 `remote` 설정을 다시 확인합니다.

```bash
git remote -v
```

출력에 `git@github.com-pdj:{user_name}/{repo_name}.git`가 보이면 성공적으로 설정된 것입니다.

### 4. **SSH 연결 테스트**

마지막으로, SSH 연결이 제대로 되는지 확인하려면 GitHub과 SSH 연결 테스트를 수행합니다.

```bash
ssh -T git@github.com-pdj
```

정상적으로 연결된다면 "Hi <username>! You've successfully authenticated..." 메시지가 나올 것입니다.

이제 B 레포도 `github.com-pdj` 형태로 설정된 SSH 키를 통해 `git push`가 정상적으로 작동할 것입니다.