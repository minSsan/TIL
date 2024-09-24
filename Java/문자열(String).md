# 자바 문자열

> ### StringBuffer vs StringBuilder
>
> 불변(Immutable)객체 가변(mutable)객체를 이해한다.  
> 불변 객체 연산의 성능 이슈를 이해한다.

### 자바 문자열 객체

- String
- StringBuffer
- StringBuilder

### 1. String 객체

> 자바의 `String` 객체는 **immutable** 객체이다.

다음과 같은 연산을 수행하게 되면 기존 String 객체의 주소가 아닌 새로운 주소를 참조하게 된다.

```java
String a = "hello";
a += " world!";

System.out.println(a); // hello world!
```

그리고 이전에 사용했던 메모리 영역은 GC 대상이 된다. 즉, 위와 같은 문자열 값 변화가 자주 발생할수록 GC 대상이 증가하며 성능저하로 이어진다.

> ### String 객체가 불변성을 갖는 이유
>
> - **보안**
>   - 값이 임의로 변경되는 것을 막을 수 있다. _(ex. 악의적인 의도로 id, pw 값을 임의로 변경하는 상황을 대비할 수 있음)_
> - **캐싱**
>   - 불변 값이므로 한번 사용한 값을 다른 곳에서도 재사용할 수 있음 _(힙 메모리 공간 절약 가능)_
> - **동기화**
>   - 불변 값이므로 여러 스레드에서 안정적으로 동시 접근이 가능하다.

### 2. StringBuffer / StringBuilder

#### 공통점

> 자바의 `StringBuffer` 및 `StringBuilder`는 모두 **mutable** 객체이다.

👉🏻 `append`, `delete` 등의 api로 문자열 값을 수정할 수 있다. 특히, 문자열 길이가 처음 정의한 버퍼 크기를 넘어가는 경우, _버퍼의 크기를 알아서 늘려준다._

- (참고) - **StringBuilder**와 **StringBuffer**는 `equals` 메서드를 오버라이딩하지 않음

  ```java
  StringBuilder a = new StringBuilder("abc");
  StringBuilder b = new StringBuilder("abc");

  System.out.println(a == b); // false
  System.out.println(a.equals(b)); // false
  System.out.println(a.toString().equals(b.toString())); // true
  ```

#### 차이점

> `StringBuffer`는 동기화를 지원하여 **멀티 스레드에서 안전**하나, `StringBuilder`는 **멀티 스레드에 안전하지 않다**.

일반적으로 성능은 `StringBuilder`가 더 좋다.
