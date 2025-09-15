## 테스트 코드 디버깅에 도움이 되는 기능들

우선적으로 아래의 기능들이 있다.

- screen.logTestingPlaygroundURL()
- screen.debug()
- logRoles

#### logTestingPlaygroundURL()

**[용도](https://testing-library.com/docs/dom-testing-library/api-debugging/#screenlogtestingplaygroundurl)**

테스트 플레이그라운드 페이지를 띄워서 가상dom의 형태를 확인할 수 있고, 실제로 targeting 이 어떻게 되는지 확인을 해볼 수 있다.

사용법은 아래와 같다.

```
import { screen } from '@testing-library/dom'

screen.logTestingPlaygroundURL()
```

#### logRoles

**[용도](https://testing-library.com/docs/dom-testing-library/api-debugging/#logroles)**

logRoles에 넘어간 dom nodes을 분석하고 role 기반의 node들 목록을 뽑아준다. Role 기반의 targeting을 할때 유용하게 사용된다.

사용 법은 아래와 같다.

```
import { logRoles } from '@testing-library/dom'

const { container } = render(<Components {...props} />)

logRoles(container)
```

#### screen.debug()

**[용도](https://testing-library.com/docs/dom-testing-library/api-debugging/#screendebug)**

console.log(prettyDOM()) 의 짧은 형태의 기능이다. dom 의 구조를 상세하게 찍어준다.

사용법은 아래와 같다

```
import { screen } from '@testing-library/dom'

screen.debug()
```
