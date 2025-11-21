## 줄바꿈

```css
/* 1. 줄바꿈 관련 예시 */

.wrap-normal {
  white-space: normal;
}

/* 부모의 영역을 무시하고 벗어난다. */
.wrap-nowrap {
  white-space: nowrap;
}

/* 
  부모의 영역을 무시하고 벗어난다. 
  줄바꿈, 띄워쓰기가 반영이 된다.
*/
.wrap-pre {
  white-space: pre;
}

/* 
  부모의 영역 안에서
  줄바꿈, 띄워쓰기가 반영이 된다.
*/
.wrap-pre-wrap {
  white-space: pre-wrap;
}

/* 
  부모의 영역 안에서
  줄바꿈, 띄워쓰기가 반영이 된다.
*/
.word-break-all {
  word-break: break-all;
}

/* 한글 어절단위로 줄바꿈 */
.word-keep-all {
  word-break: keep-all;
}

.overflow-wrap-break-word {
  overflow-wrap: break-word;
}
```

## 말줄임

```css
.ellipsis-one-line {
  white-space: nowrap;

  overflow: hidden;

  text-overflow: ellipsis;
}

.ellipsis-multi-line {
  display: -webkit-box;

  line-clamp: 3;

  -webkit-line-clamp: 3; /* 3줄까지 표시 */

  -webkit-box-orient: vertical;

  overflow: hidden;

  text-overflow: ellipsis;
}
```
