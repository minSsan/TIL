# **Hydrate**

> 멋사 장기 프로젝트를 진행하다가 `hydrate` 오류가 발생했다.  
> 스크롤 값이 변화함에 따라, 스크롤 이벤트 핸들러를 지정해주는 애니메이션을 적용할 DOM에 접근하는 과정 _(useRef 사용)_ 에서 오류가 발생한 것으로 추측된다.  
> 따라서, 오류의 원인을 파악하고자 `hydrate`를 공부하고자 한다.

<br>

# ⭐️ **Pre-render, Hydration**

hydrate 오류가 발생하는 이유를 알아보기에 앞서, 우선 `Next`의 `Pre-render` 개념과 `Hydration` 개념을 이해하고 있어야 한다.

## 👉🏻 **Pre-render**

> [출처: next 공식문서 (pre-rendering)](https://nextjs.org/learn/basics/data-fetching/pre-rendering)  
> _**By default, Next.js pre-renders every page.** This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. Pre-rendering can result in better performance and SEO._
>
> _Each generated HTML is associated with minimal JavaScript code necessary for that page. When a page is loaded by the browser, its JavaScript code runs and makes the page fully interactive. (This process is called hydration.)_

`next`에서는 따로 설정을 해주지 않는 한, 기본적으로 모든 페이지를 `pre-rendering` 한다. 즉, `next` 에서는

- 모든 페이지의 JS 코드를 HTML로 **미리 변환**하여 서버에 저장해둔다.
- 페이지가 처음 로드 될 때, 클라이언트에서 해당 페이지의 JS 파일 작업을 모두 완료하기 전까지 **서버에서 미리 생성해둔 HTML 코드**를 브라우저에 띄우는 것이다. 즉, `SSR`로 동작하는 것이다.

<br>

## 👉🏻 **Hydration**

그리고 클라이언트 사이드에서 JS 작업을 모두 마치면, 클라이언트 사이드의 JS 코드가 실행되면서 **완전한 페이지**를 구성할 수 있게 되는 것이다.  
바로 이 과정을 `hydration` 이라고 한다.

> _여기서 말하는 완전한 페이지란, **페이지 간의 이동 등의 기능들**이 모두 동작하는 페이지를 말하는 것 같다._
>
> _기본적으로 리액트는 `CSR`로 동작하도록 설계가 되어있다. 이는 대표적으로 리액트 JSX 코드에서 페이지간 이동이 발생할 때, 서버에게 페이지를 직접 요청하는 것이 아니라 JSX 코드 내부에서 `router`를 사용하여 **서버 요청 없이** 페이지를 이동하는 것을 예로 들 수 있다._
>
> 따라서, 페이지가 처음 로드되고 클라이언트 사이드의 JSX 코드가 완전히 변환되기 전까지는, 페이지 이동 버튼이 제대로 동작하지 않을 수도 있다 _(-> 그리고 이는 next에서 **next/link**를 사용하는 이유이다. **next/link**의 **`Link 태그`에 등록된 경로**는 **pre-rendering** 되므로 해당 문제점을 해결할 수 있기 때문이다)_. [👉🏻 참고 포스팅](https://hun1846.medium.com/next-js-link-db4a183a68a0)

<br>

# 🌟 **React Hydration Error**

그렇다면 리액트에서 Hydration 에러가 발생하는 이유는 무엇인지 알아보자.

> 위에서 설명했듯, `hydration`은 pre-rendering한 HTML 코드와 클라이언트의 ReactJS 코드를 서로 매칭하는 과정이다.  
> 이러한 `hydration`이 정상적으로 동작하려면 **<U>⭐️`first rendering`의 결과와 `pre-rendering`의 결과가 서로 동일해야 한다.⭐️</U>**

<br>

## **1️⃣ First Rendering과 Pre-Rendering이 매칭되지 않을 때**

<br>

예제 코드로 알아보자  
↓ 아래는 **hydration error**를 발생시키는 컴포넌트 코드다  
[출처: Next 공식문서 - react hydration error](https://nextjs.org/docs/messages/react-hydration-error)

```jsx
function MyComponent() {
  // This condition depends on `window`. During the first render of the browser the `color` variable will be different
  const color = typeof window !== "undefined" ? "red" : "blue";
  // As color is passed as a prop there is a mismatch between what was rendered server-side vs what was rendered in the first render
  return <h1 className={`title ${color}`}>Hello World!</h1>;
}
```

위 코드에서 보면, `window`의 타입이 무엇인지에 따라서 **_color 상수값이 서로 다르게 저장되는 것_** 을 확인할 수 있다. 그런데 이 `window` 객체는 브라우저에 존재하는 객체로서, 서버에서는 해당 객체가 정의되어있지 않다.

따라서 `pre-rendering(SSR)` 하는 과정에서는 color 상수의 값이 "**red**"로 저장이 되고, 클라이언트에서 `first rendering(CSR)`을 하는 과정에서는 "**blue**"로 저장이 될 것이다.

따라서 각각 _h1 태그를 렌더링할 때_ **클래스 명이 서로 다르게 지정** 될 것이다. 즉, **_pre-rendering한 HTML 결과_** 와 **_first rendering_** 한 결과가 서로 매칭되지 않으므로, 이 경우에는 `hydration error`가 발생한다.

<br>

↓ 따라서, 위의 코드를 아래 코드로 변경해야 정상적인 _hydration_ 작업이 가능해진다.

```jsx
import { useEffect, useState } from "react";
function MyComponent() {
  const [color, setColor] = useState("blue");

  useEffect(() => setColor("red"), []);

  return <h1 className={`title ${color}`}>Hello World!</h1>;
}
```

여기서는 `useEffect`를 사용하여 문제를 해결하였다.

useEffect의 effect는 **_실제 브라우저에 컴포넌트가 마운트될 때_** 실행되므로, **클라이언트 사이드**에서 실행될 수 있다. 즉, pre-rendering 과정에서는 effect가 수행될 수 없다.

따라서 `pre-rendering` 과정에서는 color 값을 default값인 '**blue**'로 설정이 된다.

그리고 ReactJS 의 `first rendering` 에서도, 컴포넌트가 마운트되기 전까지는 color 값이 default값인 '**blue**'로 설정이 된다. 그러다가 컴포넌트가 마운트되면 'red'로 바뀌는 것이다.

이렇게 되면 **_pre-rendering의 엘리먼트 구조_** 와 **_first-rendering의 엘리먼트 구조_** 가 서로 매칭되므로 hydration이 정상적으로 작동하게 된다.

<br>

## 2️⃣ HTML 태그를 남용하여 사용한 경우

<br>

Next 공식 문서에서 HTML 태그를 남용하는 경우에도 `hydrate error`가 발생한다고 한다.

**↓ 잘못된 코드**

```jsx
export const IncorrectComponent = () => {
  return (
    <p>
      <div>
        This is not correct and should never be done because the p tag has been
        abused
      </div>
      <Image src="/vercel.svg" alt="" width="30" height="30" />
    </p>
  );
};
```

**↓ 올바른 코드**

```jsx
export const CorrectComponent = () => {
  return (
    <div>
      <div>
        This is correct and should work because a div is really good for this
        task.
      </div>
      <Image src="/vercel.svg" alt="" width="30" height="30" />
    </div>
  );
};
```

나도 이번 프로젝트에서 `<p>` 태그 안에 `<span>` 태그를 넣었다가 `hydrate error`를 마주친 경험이 있다.  
이 경우에는 HTML 태그를 남용하지 않고 잘 사용하면 고칠 수 있다.

<br>

# ✏️ **ReactDOM의 `render` 메소드와 `hydrate` 메소드**

참고로 `hydrate` 는 Next 에서만 동작하는 것이 아니라, `ReactDOM` 에서 동작한다.  
아래 포스팅들을 읽어보면 도움이 될 것 같다.

- [Understanding Hydration in React applications(SSR)](https://blog.saeloun.com/2021/12/16/hydration.html)
- [hydrate란? - 한글 포스팅](https://helloinyong.tistory.com/315)

<br>

# 📝 오류 살펴보기

> 이제 관련 내용에 대해 이해했으니, 내 코드에서 발생했던 오류를 살펴봐야 한다.
>
> 사실 오류가 났던 코드를 이미 지워버린 터라, 기억에 의존해서 최대한 써보았다.

<br>

↓ 코드

```jsx
const Recruiting = forwardRef((props, ref) => {
  // ...
  const [textHeight, setTextHeight] = useState(0);

  setTextHeight(phrasesRef.current.scrollHeight / Texts.length ?? 0);
  // ...

  return (
    <div
      ref={phrasesContainerRef}
      className="..."
      style={{
        height: `${textHeight}px`,
      }}
    >
      {/* ... */}
      <span ref={phrasesRef} className="...">
        {/* ... */}
      </span>
      {/* ... */}
    </div>
  );
});
```

> 근데 지금 생각해보니 hydrate error는 p 태그 안에 span 태그를 넣어서 떴던 것 같다 ..  
> 어쨌든 위 코드도 `hydrate error`는 아니지만, **_phraseRef.current가 undefined_** 라는 오류가 발생한다.

<br>

일단 클라이언트에서 실행되는 코드 상에는 엘리먼트에 ref를 잘 설정했기 때문에, **개발자 도구**에서 next 서버가 **_pre-rendering_** 한 `HTML파일`을 확인했다.

그리고 해당 엘리먼트에 **_ref 값이 할당되어 있지 않은 것_** 을 확인할 수 있었다.

따라서, ref를 참조하여 엘리먼트의 스타일을 바꾸는 작업은 클라이언트에서만 작업할 수 있도록 하기 위해, `useLayoutEffect`로 문제를 해결하였다.

```jsx
useLayoutEffect(() => {
  setTextHeight(phrasesRef.current.scrollHeight / Texts.length ?? 0);
}, [windowDimensions.width, windowDimensions.height]);
```

> 여기서 `useEffect` 대신 `useLayoutEffect`를 사용한 이유는, **textHeight** 값이 **_특정 요소의 높이에 영향_** 을 주기 때문이다.  
> 따라서, 브라우저에 paint 하기 전에 **textHeight** 값이 확정될 수 있도록 `useLayoutEffect`를 사용하였다.

<br>

# 📖 추가 자료 포스팅 링크

- [블로그 포스팅 - hydrate 과정 자세히 기재](https://www.howdy-mj.me/next/hydrate)
