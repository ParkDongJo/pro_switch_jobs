현재 트렌드는 named export 사용
- 번들러에 최적화


문서에서는
- default export 대표 컴포넌트
- named export 그 외 컴포넌트


가독성과 재사용성을 위해, export 로 모듈화를 은근 권유하고 있음

----
가끔 `.js`와 같은 파일 확장자가 없을 때도 있습니다.

```
import Gallery from './Gallery';
```

React에서는 `'./Gallery.js'` 또는 `'./Gallery'` 둘 다 사용할 수 있지만 전자의 경우가 [native ES Modules](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules) 사용 방법에 더 가깝습니다.

----


