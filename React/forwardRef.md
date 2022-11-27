# forwardRef

🗓 22.11.26

> Next에서 특정 DOM에 접근하기 위해 습관적으로 `document.getElementById` 와 같이, 자바스크립트의 DOM 문법을 그대로 사용했다. 하지만 리액트에서 이 메소드를 사용하면서 <U>**리액트에서 제공하는 DOM 접근 방식**</U>을 활용하지 못하는 것 같아 관련 자료들을 찾게 되었다.

<br>

리액트에서는 엘리먼트에 접근할 때, `useRef hook`과 `ref prop`을 사용한다. 그렇다면 <U>_**자식 컴포넌트 내부에 있는 엘리먼트에 접근**_</U> 하고자 할 때 어떻게 해야할까?

기존 문법 그대로 **자식 컴포넌트**에 `ref prop`을 할당하면 오류가 발생한다. 왜냐하면 컴포넌트의 `props`에 `ref prop`이 존재하지 않기 때문이다. `ref`는 key와 같이 _특수한 prop_ 이기 때문에 일반적인 방법으로는 사용할 수 없다.

이렇게 특수한 `ref`를 **자식 컴포넌트 props**로 넘겨줌으로써, <U>**부모 컴포넌트에서 자식 컴포넌트 내부 엘리먼트에 접근할 수 있는 것**</U>이 바로 `forwardRef` 다.

<br>

## forwardRef 사용하기

[👉🏻 공식문서 링크](https://ko.reactjs.org/docs/forwarding-refs.html)  
번역된 리액트 공식 문서가 있긴 하지만, 가독성이 좋지 않아서 블로그 포스팅을 몇 가지 찾아보았다.

- ✔️ [블로그 포스팅 (영문)](https://dev.to/collegewap/how-to-call-the-child-component-function-from-the-parent-component-in-react-3559)
- ✔️ [블로그 포스팅 (한글)](https://www.daleseo.com/react-forward-ref/)

> 일단 내 경우는, 프로젝트에서 **애니메이션**을 관리하기 위해 _<U>부모 컴포넌트에서 자식 컴포넌트의 스크롤 높이 값</U>_ 을 가져와야했다.
>
> 따라서 자식 컴포넌트를 `forwardRef`로 감싸서 _**부모 컴포넌트가 자식 컴포넌트 내부 엘리먼트에 접근할 수 있도록**_ 해야한다.
>
> 앞에서 언급했듯 `document` 객체를 사용할 수도 있지만, 리액트를 사용하는 만큼 `ref`를 활용하는 방법을 사용해보고자 한다.

<br>

### 👉🏻 자식 컴포넌트의 scroll 높이를 필요로 하는 부모 컴포넌트 - `index.js`

```jsx
import { Main } from "../components/main";
import dynamic from "next/dynamic";
import { useEffect, useState, useRef } from "react";

const Section1NoSSR = dynamic(
  () => import("../components/section1").then((module) => module.Section1),
  {
    ssr: false,
  }
);

export default function Home() {
  const mainRef = useRef();

  const [scrollMin, setScrollMin] = useState();

  useEffect(() => {
    setScrollMin(mainRef.current.scrollHeight);
  }, []);

  console.log("scrollMin", scrollMin);

  return (
    <>
      <Main ref={mainRef} />
      <Section1NoSSR scrollMin={scrollMin} />
    </>
  );
}
```

> 부모 컴포넌트는 원래 `useRef 훅`을 사용했던 형태와 동일하게, 자식 컴포넌트에 `useRef 값`을 `ref prop`에 넘겨주면 된다.

<br>

### 👉🏻 외부로부터 참조받는 엘리먼트가 있는 컴포넌트 - `main.jsx`

```jsx
import React, { forwardRef } from "react";

export const Main = forwardRef((props, ref) => {
  return (
    <div ref={ref} style={{ height: "100vh" }}>
      <h1>Main</h1>
    </div>
  );
});
```

> 외부에서 참조할 `element`가 있는 컴포넌트는 _**`forwardRef`로 감싸서 선언**_ 한다. 이렇게 `forwordRef`로 컴포넌트를 감싸면, _props와 함께 <U>ref도 함께 전달</U>받을 수 있다._
>
> 이때, 이 ref 값을 외부가 참조할 `element의 ref`로 전달하면, 이제 외부 컴포넌트에서도 해당 element에 접근 가능하다.

<br>

## ⚠️ 주의사항

리액트 공식문서를 확인하면, `forwardRef` 같은 경우에는 _**최말단 컴포넌트 (input, button ...)**_ 에서 사용하는 것을 권장한다고 설명되어 있다.

즉, 부모에서 자식 컴포넌트의 엘리먼트를 참조하기 위해 자식 컴포넌트에게 `forwardRef`를 사용했는데, 해당 자식 컴포넌트의 자식 컴포넌트에게 또 다시 `forwardRef`를 사용하는 것과 같이 <U>**연쇄되어 사용되면 좋지 않다**</U> 는 것이다.

이런식으로 _**연속해서**_ 컴포넌트 내부 엘리먼트를 외부 컴포넌트에서 참조할 수 있도록 허용하는 것은, _**컴포넌트간에 결합도를 증가**_ 시켜 <U>프로젝트의 유지보수를 어렵게 만든다</U>고 한다.
