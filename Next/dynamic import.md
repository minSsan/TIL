# Dynamic Import

🗓 **22.11.20**

> 애니메이션 공부를 하던 중, `Next JS` 로 코드를 작성하다 오류가 발생해서 관련 자료를 찾아봤다. 그러던 중 새로운 개념(?)을 발견해서 내용을 정리하고자 한다.

<br>

이번에 문제가 되었던 부분은 바로 `window`를 사용하는 부분이였다.
뷰포트의 높이를 애니메이션 적용에 사용하려고 했는데, 이 부분에서 `window is not defined` 에러가 발생했다.

에러의 원인은 <U>_Next JS의 렌더링 방식_ </U>과 연관이 있다.  
`NextJS`는 `CSR`만 사용하는 React와 달리, `SSR`도 함께 사용한다.  
바로 이 부분에서 문제가 되는 것인데, 서버에는 `window 객체`가 없기 때문에 _(window 객체는 **브라우저 내장 객체**이기 때문에 서버에서는 사용이 불가능하다 - [window 객체 정리(한글)](https://kssong.tistory.com/29))_

`Dynamic Import`를 사용하면, 브라우저에 처음으로 컴포넌트를 렌더링할 때 서버사이드에서 렌더링되지 않도록 설정할 수 있다.

## Next 공식 문서 살피기

[📖 Next 공식 문서 링크](https://nextjs.org/docs/advanced-features/dynamic-import)

> _Next.js supports lazy loading external libraries with import() and React components with next/dynamic. Deferred loading helps improve the initial loading performance by decreasing the amount of JavaScript necessary to render the page. Components or libraries are only imported and included in the JavaScript bundle when they're used._

<br>

> 관련 공식 문서를 보다가, `lazy loading` 이라는 용어를 발견했다.  
> 개발을 하면서 여러번 들어본 용어인데, 자세히 이해하고 있지 못한 것 같아서 관련 내용도 추후에 따로 정리를 해야겠다.

<br>

일단 문서 내용으로만 보자면, `NextJS`에서 `lazy loading`을 지원함으로써, 초기 로딩 성능을 향상시킬 수 있다고 설명되어있다. 즉, `lazy loading`은 <U>로딩 성능을 향상시키는 기술</U>인 듯 하다. _(여기서는 간단하게만 이해하고, 다음에 따로 `lazy loading`에 대해서도 학습해야겠다.)_

그리고 `NextJS`에서는 `next/dynamic` 라이브러리와 `import()` 를 사용하면 `lazy loading`을 구현할 수 있다고 설명한다.

### ✔️ With No SSR

[👉🏻 공식문서 링크](https://nextjs.org/docs/advanced-features/dynamic-import#with-no-ssr)  
문서에서 현재 내게 필요한 내용은 위 링크에 나와있는 `With No SSR` 부분이였다.

> _To dynamically load a component on the client side, you can use the ssr option to disable server-rendering. This is useful if an external dependency or component relies on browser APIs like window._

컴포넌트를 **클라이언트 사이드**에서 동적으로 로드해야 할 때 사용할 수 있다고 설명되어 있다.  
여기서 예시로 든 상황이 바로 내 상황과 일치하는 것을 볼 수 있다. _(브라우저에 종속되어있는 API - `window`를 사용하는 경우)_

<br>

## ✒️ 문제 해결

이제 공식 문서를 참고하여 문제를 해결한다.

<br>

`브라우저의 window 객체를 사용하는 컴포넌트`

```jsx
import React, { useRef } from "react";

export const Section1 = () => {
  const windowHeight = useRef();
  const title = useRef();

  // window를 사용하는 부분
  windowHeight.current = window.innerHeight;

  return (
    <div style={{ height: 3 * parseInt(windowHeight.current) }}>
      {/* ... some code ... */}
    </div>
  );
};
```

우선 컴포넌트 코드를 먼저 작성한다. 위 코드를 살펴보면 `window 객체`를 사용하는 코드를 발견할 수 있다.

이 상태에서 `Section1 컴포넌트`를 스크린 컴포넌트에 import 하여 사용하면 에러가 발생한다. _(SSR이 원인)_

따라서, 공식 문서 설명에 따라 해당 컴포넌트를 import할 때 `dynamic` 라이브러리를 사용하여 해결한다.

```jsx
import dynamic from "next/dynamic";
import { useEffect } from "react";

const Section1NoSSR = dynamic(
  () => import("../components/section1").then((module) => module.Section1),
  {
    ssr: false,
  }
);

export default function Home() {
  const onScroll = () => {
    console.log(window.scrollY);
  };
  useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => {
      window.removeEventListener("scroll", onScroll);
    };
  });
  return (
    <>
      <Main />
      <Section1NoSSR />
    </>
  );
}
```

추가로, `Section1` 컴포넌트를 export 할 때, `export default 키워드`를 사용하지 않았기 때문에, 모듈 파일에서 <U>Section1 컴포넌트를 추출하는 과정</U>을 추가했다.

_`import("../components/section1").then((module) => module.Section1)`_

<br>

# 마무리

이번에 직면한 문제는 여러 방법으로 해결할 수 있었는데, 그 중 `next/dynamic` 라이브러리를 통해 해결하는 방법에 대해서 공부하였다.

더 간단한 해결법으로는 `useEffect`를 사용하는 방법이 있었다. `useEffect` 를 사용하여 <U>컴포넌트가 마운트되는 시점 (=클라이언트 사이드에서 이루어지는 작업)</U>에 `window` 객체를 사용하는 방법이다.

`useEffect 사용하여 해결하기`

```jsx
import React, { useEffect, useRef } from "react";

export const Section1 = () => {
  const windowHeight = useRef();
  const title = useRef();

  useEffect(() => {
    windowHeight.current = window.innerHeight;
  }, []);

  return (
    <div style={{ height: 3 * parseInt(windowHeight.current) }}>
      {/* ... some code ... */}
    </div>
  );
};
```

앞에서 `dynamic` 라이브러리를 사용한 것보다 훨씬 간편하게 코드가 짜여진 것을 볼 수 있다.

하지만 내가 이 방법을 사용하지 않은 이유는, 이 방법을 사용했을 때 <U>한 가지 문제점이 발견</U>되었기 때문이다.

화면이 맨 처음 렌더링될 때, `div`의 높이가 `NaN`으로 찍히는 것을 발견하였다.  
코드를 수정하여 로컬 서버를 리렌더링하면 정상적으로 높이가 설정되는 것을 볼 수 있었지만, <U>맨 처음 화면이 렌더링 될 때</U> `windowHeight` 값이 제대로 지정되지 않았다.

`Section1 컴포넌트`

```jsx
// ...

useEffect(() => {
  windowHeight.current = window.innerHeight;
});
console.log(windowHeight.current);

// ...
```

> `Section1` 컴포넌트에서 windowHeight값을 지정하고, 컴포넌트 내부에서 이 값을 console로 출력하면 `undefined` 값이 나오는 것을 확인할 수 있었다.
>
> 생각해보니 `windowHeight`의 값을 `useRef 훅`을 사용하여 저장한 것이 원인이 된 것 같았다. `useRef 훅`을 사용하면, <U>current 값이 변경되어도 컴포넌트가 리렌더링되지 않기 때문이다.</U>
>
> 따라서 아래와 같이 windowHeight를 state 값으로 변경해주면 문제가 해결된다.

```jsx
import React, { useEffect, useState } from "react";

export const Section1 = () => {
  const [windowHeight, setWindowHeight] = useState();

  useEffect(() => {
    setWindowHeight(window.innerHeight);
  });

  return (
    <div style={{ height: 3 * parseInt(windowHeight) }}>
      {/* ... some code ... */}
    </div>
  );
};
```

하지만, **원래 작성한 코드를 유지한 상태에서** 문제를 해결하고 싶었기 때문에, 해당 방법은 채택하지 않았다.
