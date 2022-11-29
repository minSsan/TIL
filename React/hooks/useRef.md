# useRef

🗓 22.11.17

> 최근에 useRef를 사용할 일이 있었는데, 이 useRef 훅을 안 쓴지 너무 오래돼서 내용이 하나도 기억나지 않았다 ..
>
> 이참에 useRef에 대한 내용을 정리해두려 한다.

<br>

## 특징

- `DOM` 에 접근할 때 사용할 수 있다.
- `useRef` 로 참조하는 값이 변경되어도 리렌더링되지 않는다.
- 화면이 리렌더링 되더라도 값이 유실되지 않는다.

<br>

## ✔️ DOM에 접근

DOM에 접근하는 경우의 가장 보편적인 예시는 input 태그를 사용하는 경우다.  
사용자가 input 태그에 입력한 값을 가져올 때 useRef 훅을 사용할 수 있다.

```jsx
export const App = () => {
  const inputName = useRef();
  console.log(inputName.current.value); // "Hello"
  return <input ref={inputName} type="text" name="name" value={"Hello"} />;
};
```

<br>

## ✔️ 값으로 사용

컴포넌트가 리렌더링될 때마다 함수 내부에서 선언된 변수들은 초기값으로 다시 렌더링된다.  
여기서 함수 내부 변수의 값도 유지하고자 할 때 useRef 훅을 사용할 수 있다.  
해당 예제는 쉽게 떠올리기 힘들어서, [관련 블로그 포스팅](https://www.daleseo.com/react-hooks-use-ref/)을 가져왔다.

<br>

## ➕ 여러 값을 하나의 ref에 저장

> 코드를 짜다보면, 비슷한 ref 값들을 저장해야 하는 경우가 발생할 수 있다.
>
> 나같은 경우에는, 애니메이션을 적용할 엘리먼트들의 참조 값들을 저장하고자 하였다.
>
> 따라서 처음에는 아래와 같은 코드를 작성하였다.

```jsx
import { useRef } from "react";
import Main from "../components/main";
import About from "../components/about";
import Schedule from "../components/schedule";

export default function Home() {
  const mainRef = useRef();
  const aboutRef = useRef();
  const scheduleRef = useRef();

  // ...

  return (
    <div>
      <Main ref={mainRef} />
      <About ref={aboutRef} />
      <Schedule ref={scheduleRef} />
    </div>
  );
}
```

이렇게 작성하게 된다면, _**컴포넌트가 증가할 때마다 ref 들도 그에 따라 증가**_ 해야 한다.

이를 방지하기 위해 `하나의 ref`로 `여러 DOM` 을 저장할 수 있는데, 바로 **배열** ref를 활용하는 것이다.

```jsx
import { useRef } from "react";
import Main from "../components/main";
import About from "../components/about";
import Schedule from "../components/schedule";

export default function Home() {
  // ref.current를 배열로 선언
  const screenRef = useRef([]);

  // ...

  return (
    <div>
      <Main ref={(el) => (screenRef.current[0] = el)} />
      <About ref={(el) => (screenRef.current[1] = el)} />
      <Schedule ref={(el) => (scheduleRef.current[2] = el)} />
    </div>
  );
}
```

위 코드를 잘 살펴보면, ref를 `하나`만 사용한 것을 볼 수 있다. 그 대신, ref의 _**current 값을 `배열`**_ 로 선언하고, 배열의 각 요소로 _**스크린의 DOM**_ 을 저장하고 있는 것이다.

그리고 `element` 에서 `ref`를 지정할 때, `콜백함수`로 지정할 수 있다. 이때 콜**백함수의 인자**로 _**<U>자기 자신의 element</U>**_ 를 전달받을 수 있다.

따라서 위 코드처럼, 콜백함수로 _**자기 자신의 element**_ 를 전달받아 `screenRef의 current 배열`에 순서대로 저장하는 것을 볼 수 있다.
