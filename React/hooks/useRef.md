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

## DOM에 접근

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

## 값으로 사용

컴포넌트가 리렌더링될 때마다 함수 내부에서 선언된 변수들은 초기값으로 다시 렌더링된다.  
여기서 함수 내부 변수의 값도 유지하고자 할 때 useRef 훅을 사용할 수 있다.  
해당 예제는 쉽게 떠올리기 힘들어서, [관련 블로그 포스팅](https://www.daleseo.com/react-hooks-use-ref/)을 가져왔다.
