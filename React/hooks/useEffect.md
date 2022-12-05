# useEffect

🗓 22.11.17

> 최근에 애니메이션 구현에 관심이 생겨서 관련 자료를 찾아보며 공부를 하다가, `useEffect` 훅을 사용하는 코드를 발견했다.
>
> 분명 프로젝트하면서 많이 사용하고 그만큼 이해를 잘 했다고 생각했는데, 예제 코드를 보면서 `useEffect`에 대한 이해도가 많이 부족하다고 느껴 관련 내용을 찾아보고 정리하려한다.

<br>

## 📒 개념

`useEffect` 를 사용하면 컴포넌트가 **마운트** _(= 컴포넌트가 처음 나타났을 때)_, **언마운트** _(= 컴포넌트가 사라질 때)_, **업데이트** 되었을 때 _(= 특정 props가 변경될 때)_ 각각 특정 작업을 처리하도록 지정할 수 있다.

<br>

## 1️⃣ 마운트 될 때 작업 수행

useEffect의 가장 기본적인 기능은, 컴포넌트가 마운트 될 때(= 처음 등장할 때) 실행할 작업을 지정해주는 것이다.

마운트 시에 주로 하는 작업 예시는 다음과 같다. [출처 - 벨로퍼트 리액트](https://react.vlpt.us/basic/16-useEffect.html)

- props로 전달받은 값을 컴포넌트의 로컬 변수로 설정
- 외부 API 요청
- 라이브러리 사용
- `setInterval` 을 통한 반복 작업 혹은 `setTimeout` 을 통한 작업 예약

```jsx
import React, { useEffect, useState } from "react";

export const Main = () => {
  // 화면이 로딩중인지 나타내는 상태 값
  const [loading, setLoading] = useState(true)
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    console.log("Main 컴포넌트가 마운트 됨");
    fetch(`API url`)
      .then((res) => res.json())
      .then((posts) => {
        setPosts(posts)
        setLoading(false)
      }
      );
  }, []);

  return (
    {loading ? (
        <div>
            <h1>Loading...</h1>
        </div>
    ) : (
        <div>
            {posts.map((post) => (
                <li>post.title</li>
            ))}
        </div>
    )}
  )
};
```

`useEffect` 함수의 첫 번째 인자로, _마운트 시에 수행할 작업을 콜백함수로_ 지정해준다.  
두 번째 인자는 *deps 값 배열*이 들어가는데, 이때는 deps 값들이 변경될 때마다 첫 번째 인자로 넘겨준 콜백함수가 실행된다.  
**빈 배열이면 마운트 될 때만 실행된다.**

<br>

## 2️⃣ 리렌더링 될 때마다 작업 수행

만약 마운트 될 때(= 처음 등장할 때)만 실행하는 것이 아니라, _리렌더링 될 때마다_ 실행하고 싶으면 아래와 같이 두 번째 인자를 생략하면 된다.

```jsx
import React, { useEffect } from "react";

export const Main = () => {
  useEffect(() => {
    console.log("Main 컴포넌트가 리렌더링 됨");
  });
```

### ⭐️ 마운트 vs 리렌더링

> 여기서 살짝 헷갈렸던 부분이 _마운트랑 리렌더링의 차이점_ 이였다.  
> 처음에는 둘이 같은 건줄 알았는데, 서로 다른 의미를 갖는다.  
> 마운트는 컴포넌트가 **"처음 생성"** 되는 것을 의미하고, 리렌더링은 컴포넌트가 **"다시 렌더링"** 되는 것을 의미한다.
>
> _ex) `useEffect`를 사용하여 마운트 시에만 특정 작업을 수행한다고 한다면, 처음 등장할 때만 작업을 수행하고, 리렌더링 될 때는 해당 작업을 수행하지 않는다._
>
> 즉, 둘은 다른 개념이다.

<br>

## 3️⃣ 특정 값으로 인해 업데이트 될 때마다 작업 수행

앞에서 말했던 deps 값들을 **두 번째 인자 배열**에 넣으면 된다.  
_컴포넌트가 마운트될 때_, 그리고 _deps 값이 변할 때마다_ 첫 번째 인자로 전달한 콜백함수가 실행된다.

```jsx
import React, { useEffect, useState } from "react";

export const Main = () => {
  const [name, setName] = useState("");

  useEffect(() => {
    console.log(name);
    console.log("컴포넌트가 마운트 혹은 업데이트 됨");
  }, [name]);
};
```

`주의` - [출처: 벨로퍼트 리액트](https://react.vlpt.us/basic/16-useEffect.html)

> `useEffect` 안에서 사용하는 `상태`나, `props` 가 있다면, `useEffect` 의 `deps` 에 넣어주어야 합니다. 그렇게 하는게, 규칙입니다.
>
> 만약 `useEffect` 안에서 사용하는 `상태`나 `props` 를 `deps` 에 넣지 않게 된다면 `useEffect` 에 등록한 함수가 실행 될 때 `최신 props / 상태`를 가르키지 않게 됩니다.

<br>

## 4️⃣ 언마운트 될 때 작업 수행

**사라질 때(deps가 없는 경우)** 혹은 **deps 값이 업데이트 되기 직전에(deps가 있는 경우)** `useEffect` 에서 사용하는 함수의 리턴 값이 수행된다.  
`useEffect` 에 전달한 콜백함수가 반환하는 함수를 _**clean up 함수**_ 라고 부른다.

```jsx
import React, { useEffect, useState } from "react";

export const Main = () => {
  const [name, setName] = useState("");

  useEffect(() => {
    console.log(name);
    console.log("컴포넌트가 마운트 혹은 업데이트 됨");
    // cleanup 함수
    return () => {
      console.log(name);
      console.log("컴포넌트가 언마운트 됨 혹은 업데이트 직전");
    };
  }, [name]);
};
```

<br>

> 추가로, `useEffect`에서 return 으로 반환한 `clean up 함수`는 매번 effect가 실행되기 전에 호출되기도 한다. 즉, `clean up 함수`는 _**1️⃣ effect가 실행되기 전**_ 과, _**2️⃣ unmount될 때**_ 실행된다.  
> [👉🏻 참고 블로그 포스팅](https://jungpaeng.tistory.com/92)
>
> 그리고 위에 첨부한 블로그 포스팅 내용을 참고하여, 이전에 작성했던 코드를 아래와 같이 수정하기도 하였다.

<br>

`수정 전`

```jsx
import { useState } from "react";

const [todos, setTodos] = useState();

useEffect(() => {
  const todosData = JSON.parse(localStorage.getItem("todos") ?? "[]");
  setTodos(todosData);
}, []);
```

<br>

`수정 후`

```jsx
import { useState } from "react";

const [todos, setTodos] = useState();

useEffect(() => {
  const todosData = JSON.parse(localStorage.getItem("todos") ?? "[]");
  setTodos(todosData);
}, [setTodos]);
```

<br>

### ✔️ `setter`를 `deps배열`에 선언

<hr>

> `useState`를 선언할 때 **state 값**과 **setState 함수**가 할당되는데, 이때 `setState(setter 함수)`는 **매 렌더링마다 재생성되지 않는 특징을 갖고 있다.** 그리고 위의 이펙트 함수의 경우에는, 이펙트 함수 내부에서 `setTodos`만 사용하고 있기 때문에 **deps 배열**에 `setTodos`를 담아주는 것이 _좋다._
>
> 담아주어야 한다고 표현하지 않고, 담아주는 것이 좋다고 표현한 이유는, 사실 이 어플리케이션에서는 **deps**를 _빈 배열_ 로 둬도 실행할 때 큰 문제가 없기 때문이다. _(첫 렌더링 이후 `setTodos`가 변경될 일이 없기 때문이다)_
>
> 하지만, `Referentially Stable` _(참조적 안정성 - 대충 `참조 값`이 **항상 `최신 값`을 가리키는 것** 을 의미하는 것 같다)_ 을 위해서 넣어주는 것이 좋다.  
> [👉🏻 참고 포스팅](https://www.reddit.com/r/reactjs/comments/tbt2z8/do_i_need_to_setter_functions_to_the_dependency/)
