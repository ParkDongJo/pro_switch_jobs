


## DOM 과 Virtual DOM
-----



https://www.youtube.com/watch?v=1ojA5mLWts8






## 웹 접근성
------
웹 접근성은 인지, 신체 장애가 있는 사람들 조차도 쉽게 사용할 수 있는 웹 환경을 구축하는 기술을 말한다. 프론트 개발자로서 웹접근성을 높이는 방법은 아래와 같다.

- 시맨틱 HTML을 사용한다.
	- 정보의 의미에 맞게 구성한 웹을 말함
	- 태그를 의미에 맞게 사용
		- 페이지 제목 또는 글 제목에는 `<header>` 사용
		- 콘텐츠 작성자, 저작권 정보, 관련 문서 등의 내용을 포함할 때는 `<footer>` 사용
		- 문서의 핵심 주제를 포함하는 메인 콘텐츠에 `<main>` 사용
		- 독립적으로 구분해야 하는 콘텐츠에 `<article>` 사용
		- 페이지의 가장 중요한 제목으로 `<h1>` 사용
		- 순서가 없는 목록으로 `<ul>`과 `<li>` 사용
- 대체 텍스트를 제공한다.
	- 이미지, 영상 등등의 시각적 콘텐츠 제공 시, alt 속성을 활용해 콘텐츠에 대한 설명을 넣어준다.
- 접근성 지침(WCAG) 에 따른 적절한 글꼴 크기를 사용한다.
- ARIA 속성을 사용한다.
	- 남용 시 사용성에 더 큰 장벽을 만들 수 있다.
	- W3C-ARIA 스펙에 의해 정의된다. (https://www.w3.org/TR/wai-aria-1.2/)


사실 나도 단편적인 부분만 알고 있어서, React 로 프로젝트 구축 시, eslint 에 **eslint-plugin-jsx-a11y**을 적용하여 개발 시 도움을 받는 다면, 웹접근성의 품질을 어느정도 확보 할 수 있다.


https://tech.kakaopay.com/post/accessibility-stories-for-everyone/
https://medium.com/@yujso66/%EB%B2%88%EC%97%AD-%EC%9B%B9-%EC%A0%91%EA%B7%BC%EC%84%B1-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0-%ED%94%84%EB%9F%B0%ED%8A%B8%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EA%B0%80%EC%9D%B4%EB%93%9C-cdac7a1e2710


## aria-*  속성
-----
ARIA는 HTML요소에 **접근 가능한 설명용 텍스트**를 넣을 수 있다.

- aria-role
태그의 역할을 알려주는 속성

- aria-label
영역을 표현하는 경우가 아닌 이미지를 통해 영역을 표현하는 경우 해당 영역이 어떤 영역인지 설명할 수 있는 요소

- aria-labelledby
화면에 현재 요소를 **설명할 텍스트가 있을 경우**에 해당 텍스트 영역과 현재 요소를 **연결**할 때 사용

https://www.youtube.com/watch?v=MQHNTzdqet0
https://www.youtube.com/watch?v=SfnEO-mv7Yg&t=534s
https://www.youtube.com/watch?v=OzxKLC6B1wE
