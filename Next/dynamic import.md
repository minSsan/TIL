# Dynamic Import

π **22.11.20**

> μ λλ©μ΄μ κ³΅λΆλ₯Ό νλ μ€, `Next JS` λ‘ μ½λλ₯Ό μμ±νλ€ μ€λ₯κ° λ°μν΄μ κ΄λ ¨ μλ£λ₯Ό μ°Ύμλ΄€λ€. κ·Έλ¬λ μ€ μλ‘μ΄ κ°λ(?)μ λ°κ²¬ν΄μ λ΄μ©μ μ λ¦¬νκ³ μ νλ€.

<br>

μ΄λ²μ λ¬Έμ κ° λμλ λΆλΆμ λ°λ‘ `window`λ₯Ό μ¬μ©νλ λΆλΆμ΄μλ€.
λ·°ν¬νΈμ λμ΄λ₯Ό μ λλ©μ΄μ μ μ©μ μ¬μ©νλ €κ³  νλλ°, μ΄ λΆλΆμμ `window is not defined` μλ¬κ° λ°μνλ€.

μλ¬μ μμΈμ <U>_Next JSμ λ λλ§ λ°©μ_ </U>κ³Ό μ°κ΄μ΄ μλ€.  
`NextJS`λ `CSR`λ§ μ¬μ©νλ Reactμ λ¬λ¦¬, `SSR`λ ν¨κ» μ¬μ©νλ€.  
λ°λ‘ μ΄ λΆλΆμμ λ¬Έμ κ° λλ κ²μΈλ°, μλ²μλ `window κ°μ²΄`κ° μκΈ° λλ¬Έμ _(window κ°μ²΄λ **λΈλΌμ°μ  λ΄μ₯ κ°μ²΄**μ΄κΈ° λλ¬Έμ μλ²μμλ μ¬μ©μ΄ λΆκ°λ₯νλ€ - [window κ°μ²΄ μ λ¦¬(νκΈ)](https://kssong.tistory.com/29))_

`Dynamic Import`λ₯Ό μ¬μ©νλ©΄, λΈλΌμ°μ μ μ²μμΌλ‘ μ»΄ν¬λνΈλ₯Ό λ λλ§ν  λ μλ²μ¬μ΄λμμ λ λλ§λμ§ μλλ‘ μ€μ ν  μ μλ€.

## Next κ³΅μ λ¬Έμ μ΄νΌκΈ°

[π Next κ³΅μ λ¬Έμ λ§ν¬](https://nextjs.org/docs/advanced-features/dynamic-import)

> _Next.js supports lazy loading external libraries with import() and React components with next/dynamic. Deferred loading helps improve the initial loading performance by decreasing the amount of JavaScript necessary to render the page. Components or libraries are only imported and included in the JavaScript bundle when they're used._

<br>

> κ΄λ ¨ κ³΅μ λ¬Έμλ₯Ό λ³΄λ€κ°, `lazy loading` μ΄λΌλ μ©μ΄λ₯Ό λ°κ²¬νλ€.  
> κ°λ°μ νλ©΄μ μ¬λ¬λ² λ€μ΄λ³Έ μ©μ΄μΈλ°, μμΈν μ΄ν΄νκ³  μμ§ λͺ»ν κ² κ°μμ κ΄λ ¨ λ΄μ©λ μΆνμ λ°λ‘ μ λ¦¬λ₯Ό ν΄μΌκ² λ€.

<br>

μΌλ¨ λ¬Έμ λ΄μ©μΌλ‘λ§ λ³΄μλ©΄, `NextJS`μμ `lazy loading`μ μ§μν¨μΌλ‘μ¨, μ΄κΈ° λ‘λ© μ±λ₯μ ν₯μμν¬ μ μλ€κ³  μ€λͺλμ΄μλ€. μ¦, `lazy loading`μ <U>λ‘λ© μ±λ₯μ ν₯μμν€λ κΈ°μ </U>μΈ λ― νλ€. _(μ¬κΈ°μλ κ°λ¨νκ²λ§ μ΄ν΄νκ³ , λ€μμ λ°λ‘ `lazy loading`μ λν΄μλ νμ΅ν΄μΌκ² λ€.)_

κ·Έλ¦¬κ³  `NextJS`μμλ `next/dynamic` λΌμ΄λΈλ¬λ¦¬μ `import()` λ₯Ό μ¬μ©νλ©΄ `lazy loading`μ κ΅¬νν  μ μλ€κ³  μ€λͺνλ€.

### βοΈ With No SSR

[ππ» κ³΅μλ¬Έμ λ§ν¬](https://nextjs.org/docs/advanced-features/dynamic-import#with-no-ssr)  
λ¬Έμμμ νμ¬ λ΄κ² νμν λ΄μ©μ μ λ§ν¬μ λμμλ `With No SSR` λΆλΆμ΄μλ€.

> _To dynamically load a component on the client side, you can use the ssr option to disable server-rendering. This is useful if an external dependency or component relies on browser APIs like window._

μ»΄ν¬λνΈλ₯Ό **ν΄λΌμ΄μΈνΈ μ¬μ΄λ**μμ λμ μΌλ‘ λ‘λν΄μΌ ν  λ μ¬μ©ν  μ μλ€κ³  μ€λͺλμ΄ μλ€.  
μ¬κΈ°μ μμλ‘ λ  μν©μ΄ λ°λ‘ λ΄ μν©κ³Ό μΌμΉνλ κ²μ λ³Ό μ μλ€. _(λΈλΌμ°μ μ μ’μλμ΄μλ API - `window`λ₯Ό μ¬μ©νλ κ²½μ°)_

<br>

## βοΈ λ¬Έμ  ν΄κ²°

μ΄μ  κ³΅μ λ¬Έμλ₯Ό μ°Έκ³ νμ¬ λ¬Έμ λ₯Ό ν΄κ²°νλ€.

<br>

`λΈλΌμ°μ μ window κ°μ²΄λ₯Ό μ¬μ©νλ μ»΄ν¬λνΈ`

```jsx
import React, { useRef } from "react";

export const Section1 = () => {
  const windowHeight = useRef();
  const title = useRef();

  // windowλ₯Ό μ¬μ©νλ λΆλΆ
  windowHeight.current = window.innerHeight;

  return (
    <div style={{ height: 3 * parseInt(windowHeight.current) }}>
      {/* ... some code ... */}
    </div>
  );
};
```

μ°μ  μ»΄ν¬λνΈ μ½λλ₯Ό λ¨Όμ  μμ±νλ€. μ μ½λλ₯Ό μ΄ν΄λ³΄λ©΄ `window κ°μ²΄`λ₯Ό μ¬μ©νλ μ½λλ₯Ό λ°κ²¬ν  μ μλ€.

μ΄ μνμμ `Section1 μ»΄ν¬λνΈ`λ₯Ό μ€ν¬λ¦° μ»΄ν¬λνΈμ import νμ¬ μ¬μ©νλ©΄ μλ¬κ° λ°μνλ€. _(SSRμ΄ μμΈ)_

λ°λΌμ, κ³΅μ λ¬Έμ μ€λͺμ λ°λΌ ν΄λΉ μ»΄ν¬λνΈλ₯Ό importν  λ `dynamic` λΌμ΄λΈλ¬λ¦¬λ₯Ό μ¬μ©νμ¬ ν΄κ²°νλ€.

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

μΆκ°λ‘, `Section1` μ»΄ν¬λνΈλ₯Ό export ν  λ, `export default ν€μλ`λ₯Ό μ¬μ©νμ§ μμκΈ° λλ¬Έμ, λͺ¨λ νμΌμμ <U>Section1 μ»΄ν¬λνΈλ₯Ό μΆμΆνλ κ³Όμ </U>μ μΆκ°νλ€.

_`import("../components/section1").then((module) => module.Section1)`_

<br>

# λ§λ¬΄λ¦¬

μ΄λ²μ μ§λ©΄ν λ¬Έμ λ μ¬λ¬ λ°©λ²μΌλ‘ ν΄κ²°ν  μ μμλλ°, κ·Έ μ€ `next/dynamic` λΌμ΄λΈλ¬λ¦¬λ₯Ό ν΅ν΄ ν΄κ²°νλ λ°©λ²μ λν΄μ κ³΅λΆνμλ€.

λ κ°λ¨ν ν΄κ²°λ²μΌλ‘λ `useEffect`λ₯Ό μ¬μ©νλ λ°©λ²μ΄ μμλ€. `useEffect` λ₯Ό μ¬μ©νμ¬ <U>μ»΄ν¬λνΈκ° λ§μ΄νΈλλ μμ  (=ν΄λΌμ΄μΈνΈ μ¬μ΄λμμ μ΄λ£¨μ΄μ§λ μμ)</U>μ `window` κ°μ²΄λ₯Ό μ¬μ©νλ λ°©λ²μ΄λ€.

`useEffect μ¬μ©νμ¬ ν΄κ²°νκΈ°`

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

μμμ `dynamic` λΌμ΄λΈλ¬λ¦¬λ₯Ό μ¬μ©ν κ²λ³΄λ€ ν¨μ¬ κ°νΈνκ² μ½λκ° μ§μ¬μ§ κ²μ λ³Ό μ μλ€.

νμ§λ§ λ΄κ° μ΄ λ°©λ²μ μ¬μ©νμ§ μμ μ΄μ λ, μ΄ λ°©λ²μ μ¬μ©νμ λ <U>ν κ°μ§ λ¬Έμ μ μ΄ λ°κ²¬</U>λμκΈ° λλ¬Έμ΄λ€.

νλ©΄μ΄ λ§¨ μ²μ λ λλ§λ  λ, `div`μ λμ΄κ° `NaN`μΌλ‘ μ°νλ κ²μ λ°κ²¬νμλ€.  
μ½λλ₯Ό μμ νμ¬ λ‘μ»¬ μλ²λ₯Ό λ¦¬λ λλ§νλ©΄ μ μμ μΌλ‘ λμ΄κ° μ€μ λλ κ²μ λ³Ό μ μμμ§λ§, <U>λ§¨ μ²μ νλ©΄μ΄ λ λλ§ λ  λ</U> `windowHeight` κ°μ΄ μ λλ‘ μ§μ λμ§ μμλ€.

`Section1 μ»΄ν¬λνΈ`

```jsx
// ...

useEffect(() => {
  windowHeight.current = window.innerHeight;
});
console.log(windowHeight.current);

// ...
```

> `Section1` μ»΄ν¬λνΈμμ windowHeightκ°μ μ§μ νκ³ , μ»΄ν¬λνΈ λ΄λΆμμ μ΄ κ°μ consoleλ‘ μΆλ ₯νλ©΄ `undefined` κ°μ΄ λμ€λ κ²μ νμΈν  μ μμλ€.
>
> μκ°ν΄λ³΄λ `windowHeight`μ κ°μ `useRef ν`μ μ¬μ©νμ¬ μ μ₯ν κ²μ΄ μμΈμ΄ λ κ² κ°μλ€. `useRef ν`μ μ¬μ©νλ©΄, <U>current κ°μ΄ λ³κ²½λμ΄λ μ»΄ν¬λνΈκ° λ¦¬λ λλ§λμ§ μκΈ° λλ¬Έμ΄λ€.</U>
>
> λ°λΌμ μλμ κ°μ΄ windowHeightλ₯Ό state κ°μΌλ‘ λ³κ²½ν΄μ£Όλ©΄ λ¬Έμ κ° ν΄κ²°λλ€.

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

νμ§λ§, **μλ μμ±ν μ½λλ₯Ό μ μ§ν μνμμ** λ¬Έμ λ₯Ό ν΄κ²°νκ³  μΆμκΈ° λλ¬Έμ, ν΄λΉ λ°©λ²μ μ±ννμ§ μμλ€.
