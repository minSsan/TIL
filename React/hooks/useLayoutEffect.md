# useLayoutEffect

🗓 22.11.24

> 프론트엔드 개발자로 성장하기 위해 이것저것 공부하다보니, 이제 `성능`에 대해서도 고민을 하기 시작했다.  
> 그냥 화면을 예쁘게 보여주기 위한 것만이 중요한 게 아니라, 성능을 최적화하여 효율적으로 코드를 동작시키는 것 또한 개발자로서 중요한 역량이자 차별점이라고 생각한다.
>
> 그러다가 몇몇 블로그 포스팅에서 `useEffect`와 `useLayoutEffect`를 병행하여 사용하는 것을 발견했다. 어떤 경우에 `useEffect`를 대신하여 `useLayoutEffect`를 사용하는 것이 좋은지 기억해두기 위해, 관련 내용을 정리하고자 하였다.
> 👉🏻 [참고 포스팅 링크](https://medium.com/@jnso5072/react-useeffect-%EC%99%80-uselayouteffect-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-e1a13adf1cd5)

<br>

## 📖 브라우저 렌더링 과정

> 일단 둘의 차이점을 이해하기 위해서는 브라우저가 어떤 과정으로 렌더링되는지 이해하고 있어야 한다.  
> 관련 내용을 [개인 노션](https://puzzling-bridge-fe7.notion.site/a6dc567ae1904173ae44b07f093b7b51)에 기록하였지만, 내용을 많이 잊어버린 것 같아서 관련해서 다시 공부를 해야할 것 같다.
>
> 노션 내용을 모두 잘 숙지하고 있다면 더 좋긴 하지만, 일단 `render` 와 `paint` 의 차이만 알고 있어도 `useEffect`와 `useLayoutEffect`의 차이점을 충분히 이해할 수 있을 것 같다.

<br>

### 👉🏻 Render

정확하게 이 `렌더 과정`은 브라우저에 요소를 배치하기 위해 _**DOM을 생성하는 과정**_ 이다. 쉽게 말해서 <U>**브라우저에 DOM을 띄우기 위한 계산 과정**</U>인 것이다.

### 👉🏻 Paint

앞서 생성한 DOM을 토대로, <U>**브라우저 화면에 요소를 "그리는" 과정**</U>이다.

<br>

## 🌟 useEffect vs useLayoutEffect

> 둘의 차이점은 `1️⃣ effect가 호출되는 시점`과 `2️⃣ effect가 동기/비동기적으로 실행되는지`에 있다.

<br>

### ✔️ useEffect - paint 이후

`useEffect`에서는 effect가 <U>**paint 이후**</U>에 <U>**비동기적**</U>으로 실행된다.

### ✔️ useLayoutEffect - render 이후 ~ paint 이전

`useLayoutEffect`에서는 effect가 <U>**render가 완료된 이후부터 동기적으로**</U> 실행된다. 즉, _effect 실행이 끝나기 전까지는 사용자는 화면에서 요소를 볼 수 없다._

<br>

이러한 특징 때문에, react 공식 문서에서도 웬만하면 `useEffect` 사용을 권장한다.

다만, _화면을 맨 처음 보여주기 전_ 에 **계산 과정**을 거쳐서, _**해당 결과값에 따라 엘리먼트 값이 달라진다**_ 면, `useLayoutEffect`를 사용할 수 있다.

이 경우에는 엘리먼트 요소가 **계산 결과 값에 영향을 받**기 때문에, <U>엘리먼트 요소를 보여주기(`Paint 과정`) 전에 계산을 확실하게 다 끝내는 것</U>이다.

<br>

# 번외

> Next로 스크롤 애니메이션을 구현하는 것을 공부하다가, scroll 이벤트의 관련 포스팅을 몇 개 발견했다.
>
> [👉🏻 scroll 이벤트 렌더링 최적화](https://goddino.tistory.com/312)  
> [👉🏻 Redux로 scroll 위치 기억하기](https://sso-feeling.tistory.com/550)
>
> 이 과정에서 useLayoutEffect에 대한 내용도 발견하여 이번에 정리하게 된 것인데, 다음에는 **상태 관리 툴(ex. 리덕스)** 과 **성능 최적화** 와 관련한 내용도 공부하고싶다.
