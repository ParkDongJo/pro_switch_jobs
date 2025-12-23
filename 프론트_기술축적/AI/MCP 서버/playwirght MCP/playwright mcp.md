

Playwright의 MCP(Manual Control Panel) 또는 Playwright의 Interactive/Manual Mode(예: npx playwright codegen, VSCode Playwright extension, 또는 Playwright Test Runner의 "Show Browser" 기능 등)에서는 storageState(인증 세션)을 직접 적용할 수 있는 방법이 있습니다.

### 1. Playwright Codegen에서 storageState 적용

npx playwright codegen 명령어로 수동 브라우저를 띄울 때, 인증 세션을 적용하려면 다음과 같이 실행합니다:

bash

Apply to vehicles.spe...

Run

npx playwright codegen --storage-state=playwright/.auth/user.json https://172.28.5.93:8443/web/client/vehicles/list

- 이렇게 하면 해당 인증 세션이 적용된 상태로 브라우저가 열립니다.

- 이후 수동으로 UI를 조작하거나, 코드 스니펫을 생성할 수 있습니다.

---

### 2. Playwright Test에서 storageState 적용

테스트 코드에서 storageState를 적용하려면 이미 사용하신 것처럼 test.use({ storageState: ... })를 사용합니다.

---

### 3. VSCode Playwright Extension에서 storageState 적용

VSCode의 Playwright extension을 사용할 경우, .env 또는 playwright.config.ts에서 storageState를 지정해두면, "Show Browser"로 띄울 때도 인증이 적용됩니다.

---

### 4. Playwright Test Runner의 "Show Browser" (UI 모드)

bash

Apply to vehicles.spe...

Run

npx playwright test --ui

- 이 모드에서 playwright.config.ts에 storageState가 지정되어 있으면, UI로 띄운 브라우저에도 인증이 적용됩니다.


# 유저 인증 자동화
test.use({ storageState: ... })


# 화면기반 시나리오 codegen
아래 코드를 실행하면, 브라우저가 띄워지고 개발자가 실제 실행을 하면서 화면을 사용하면, 사용 이벤트에 맞게 테스트 코드가 자동으로 생성된다.

```shell
npx playwright codegen --load-storage=playwright/.auth/user.json https://172.28.5.93:8443/web/client/vehicles/list
```

아래와 같이 테스트 러너에 타겟팅이 찍히는데, 이 테스트 러너를 닫지말고 바로바로 코드를 복사해서 나의 테스트 코드에 활용해야한다.
![[Pasted image 20250801214910.png]]

# 시도했던 [[프롬프트]]

```
@https://172.28.5.93:8443/web 로 접속하면  
로그인화면이 뜰텐데 아래 계정으로 로그인하면 돼 

아이디는 : 
비밀번호는 : 
  
로그인을 완료하고 나면, 옆 사이드바에서 차량 메뉴를 클릭하면, 차량 목록이 나와  
차량목록에서 svg.view_settingsIcon__g09ywx6 톱니바퀴 아이콘을 클릭해라 그러면 div.view_container__1tyialy0 팝업이 뜨는데, 

playwright mcp 를 활용해서 그 팝업에서 할 수있는 모든 기능을 사용해봐 
그리고 브라우저를 띄워서 너가 실행하는 모습을 실시간으로 보여줘
```


```
@https://172.28.5.93:8443/web/client/vehicles/list 로 접속해서 svg.view_settingsIcon__g09ywx6 톱니바퀴 아이콘을 클릭해라 그러면 div.view_container__1tyialy0 팝업이 뜨는데,  
  
이때 '총 주행 거리' 아이템을 drag & drop 해서 '냉동기 온도' 에 하는 시나리오를 직접 실행해봐
```

