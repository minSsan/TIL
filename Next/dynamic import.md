# Dynamic Import

🗓 **22.11.20**

> 애니메이션 공부를 하던 중, `Next JS` 로 코드를 작성하다 오류가 발생해서 관련 자료를 찾아봤다. 그러던 중 새로운 개념(?)을 발견해서 내용을 정리하고자 한다.

<br>

이번에 문제가 되었던 부분은 바로 `window`를 사용하는 부분이였다.
뷰포트의 높이를 애니메이션 적용에 사용하려고 했는데, 이 부분에서 `window is not defined` 에러가 발생했다.

에러의 원인은 _Next JS의 렌더링 방식_ 과 연관이 있다.  
`NextJS`는 `CSR`만 사용하는 React와 달리, `SSR`도 함께 사용한다.  
바로 이 부분에서 문제가 되는 것인데, 서버에는 `window 객체`가 없기 때문에 _(window 객체는 **브라우저 내장 객체**이기 때문에 서버에서는 사용이 불가능하다 - [window 객체 정리(한글)](https://kssong.tistory.com/29))_

`Dynamic Import`를 사용하면, 브라우저에 처음으로 컴포넌트를 렌더링할 때 서버사이드에서 렌더링되지 않도록 설정할 수 있다.

<hr>

[📖 Next 공식 문서 링크](https://nextjs.org/docs/advanced-features/dynamic-import)

> _Next.js supports lazy loading external libraries with import() and React components with next/dynamic. Deferred loading helps improve the initial loading performance by decreasing the amount of JavaScript necessary to render the page. Components or libraries are only imported and included in the JavaScript bundle when they're used._
