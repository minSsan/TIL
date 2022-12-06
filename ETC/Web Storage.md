# 웹 스토리지

> 브라우저에서 사용자로부터 전달받은 데이터는, 서버의 `데이터 베이스`에 저장하게 된다. 하지만 데이터의 중요도가 낮아서 **유실되어도 될 정도**의 데이터라면, 데이터 베이스에 저장하는 것보다 `Local Storage` 혹은 `Session Storage`를 사용하는 것이 더 효율적일 수 있다. 데이터 베이스의 공간을 차지하는 비용을 절약할 수 있기 때문이다.
>
> 이런 경우에 사용할 수 있는 것이 바로 `Web Storage`이다. 웹 스토리지는 `자바스크립트`로 **간단한 데이터**들을 관리할 수 있는 공간으로, **로컬 스토리지**와 **세션 스토리지**가 있는데, 각각의 차이점과 예시에 대해서 공부하였다.

<br>

## 🌟 Local Storage vs Session Storage

로컬 스토리지와 세션 스토리지 모두 _**`자바스크립트` 내에서 데이터를 조작 | 관리할 수 있다는 점**_ 과 _**웹 브라우저 상에 데이터를 저장**_ 하는 공통점을 가지고 있다. 또한, 웹 스토리지는 _**서로 동일한 origin(= 도메인, 프로토콜, 포트)끼리**_ 데이터를 공유할 수 있다.

다만, `로컬 스토리지`에서는 저장된 데이터가 **서로 다른 탭 (혹은 세션)에서 공유 가능**하다. 그리고 _**세션이 종료**되어도 로컬 스토리지에 저장된 데이터가 **유실되지 않는다.**_

그에 반해 `세션 스토리지`는 `origin` 뿐만 아니라 `브라우저 탭`에도 종속된다. 따라서 세션 스토리지에 저장된 데이터는 각 세션마다 저장되는 데이터로, 세션 스토리지에 저장된 데이터는 **서로 다른 탭끼리 공유할 수 없다.** 또한,_세션이 종료_ 되면 _**세션 스토리지**에 저장된 데이터도 함께 사라지는_ 특징을 갖는다.

<br>

## ✔️ 코드에 적용

> `NextJS`로 _Todo App_ 을 만들어보는 간단한 스터디를 진행하고 있는데, 이 과정에서 **Todo 데이터**를 저장할 때 서버 대신 `로컬 스토리지`를 사용하기로 하였다.

<br>

```tsx
import React, { useEffect, useState } from "react";

const Main = () => {
  const [todos, setTodos] = useState<TodoProps[]>([]);
  const [inputs, setInputs] = useState<Inputs>();

  useEffect(() => {
    const todosData = JSON.parse(localStorage.getItem("todos") ?? "[]");
    setTodos(todosData);
  }, [setTodos]);

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);

  return (
    <div>
      <h3>== Todo 리스트 ==</h3>
      {/* ... some code ... */}

      <h3>== Todo 추가하기 ==</h3>
      {/* ... some code ... */}

      <button type="submit" onClick={handleAddSubmit}>
        추가하기
      </button>
    </div>
  );
};

export default Main;
```

<br>

스터디에서는 `useEffect hook` 내부에서 로컬 스토리지 설정이 이뤄졌다.

- 우선, 사용자가 todo list를 바꿀 때마다, 로컬 스토리지의 todos 데이터 내역을 업데이트해야 했기 때문에 useEffect hook을 사용하였다.

- 첫 번째 이펙트 함수의 경우에는, `todos key`에 저장된 데이터가 없는 경우에 `JSON.parse()` 부분에서 에러가 발생할 수 있기 때문에, 이에 대한 예외처리를 `??` 문법으로 처리했다.

- 두 번째 이펙트 함수의 경우에는, `todos state 값`이 변경될 때마다 로컬 스토리지에 저장된 `todos 데이터 값`을 갱신시켜주는 부분이다.  
  🌟 다만, **페이지가 새로고침** 되거나, **브라우저가 종료**되면 _로컬 스토리지에 저장되었던 데이터가 모두 날아가는 오류_ 가 있어서, 해당 부분에 대한 공부가 필요할 것 같다.

<br>

## 📝 참고 포스팅

[👉🏻 참고 블로그 포스팅](https://www.daleseo.com/js-web-storage/) - 로컬 스토리지와 세션 스토리지 사용방법

[👉🏻 참고 블로그 포스팅](https://hahahoho5915.tistory.com/32) - 쿠키와 세션의 차이

[👉🏻 참고 블로그 포스팅](https://ko.javascript.info/localstorage) - 로컬 스토리지와 세션 스토리지 _+ 웹 스토리지에서 쿠키가 아닌 세션을 사용하는 이유_
