Q) styled-components와 tailwind를 사용할 때 프로젝트의 성능에 어떤 영향을 미칠 수 있나요?

- **성능 영향**: styled-components는 런타임에 스타일을 생성하여 약간의 성능 저하가 있을 수 있습니다. 반면, Tailwind CSS는 컴파일 타임에 CSS를 생성하여 성능에 덜 영향을 줍니다.
- **생산성 영향**: styled-components는 js단에서 스타일에 역할을 부여하거나 가독성을 높일 수 있다. 반면 Tailwind 는 유틸들의 조합이 길어질 시 가독성이 떨어 질 수 있다.

Q) **reset.css vs [([[normalize.css]])] 활용

- **사용하지 않았을 때의 문제**: 사용하지 않으면 브라우저 간 스타일 차이로 인해 일관성 없는 UI가 될 수 있습니다.
- **선택 기준**: 프로젝트에 일관된 기본 스타일이 필요하다면 normalize.css를, 완전한 스타일 초기화가 필요하다면 reset.css를 선택합니다.