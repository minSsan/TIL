# 💫 Heap

🗓 22.11.11

> - **이진 트리 구조**로 값을 저장한다 <br>
> - 값을 넣음과 동시에 **정렬**이 이루어진다 _(시간복잡도는 logN = 시간복잡도가 NlogN인 퀵 sort 보다 빠르다)_ <br>
> - **최대 힙**, **최소 힙** 두 종류로 나눌 수 있다

## ✔️ C++

---

c++ 에서는 **우선순위 큐 - priority_queue** 로 힙을 사용할 수 있다.

```c++
#include <iostream>
#include <queue>

using namespace std;

int main() {
    cin.tie(nullptr);
    cout.tie(nullptr);
    ios_base::sync_with_stdio(false);

    priority_queue<int> pq;

    pq.push(1);
    pq.push(3);
    pq.push(5);

    cout << pq.top() << '\n'; // 5

    return 0;
}
```

c++ 에서는 기본적으로 priority_queue 가 **최대 힙**으로 사용된다.

<br>

## ➕ 최대 힙

---

부모 노드에 저장된 값이 자식 노드에 저장된 값보다 큰 이진트리이다.

앞에서 힙 자료구조에 값을 넣을 때마다 정렬이 이루어진다고 했기 때문에, 여기서는 **최상위 부모 노드에 저장된 값이 항상 최대값**이 된다.

<br>

## ➕ 최소 힙

---

반대로 최소 힙은, 부모 노드에 저장된 값이 자식 노드에 저장된 값보다 작은 이진트리이다.

여기서도 마찬가지로, **최상위 부모 노드에 저장된 값이 항상 최소값이 된다.**

<br>

## ✔️ C++ 에서 최소 힙 사용하기

---

- 첫 번째 방법은 우선순위 큐에 **원래 값을 음수 값으로 변환하여 넣는 것**이다. <br>
  그렇게 되면 top 에 존재하는 값이 가장 큰 음수 값이 되므로, 이를 다시 양수로 변환하면 최소값이 된다.

```c++
#include <iostream>
#include <queue>

using namespace std;

int main() {
    priority_queue<int> pq;

    // 최솟값을 우선순위로
    pq.push(-1);
    pq.push(-3);
    pq.push(-2);

    cout << -pq.top(); // 1

    return 0;
}
```

<br>

- 두 번째 방법은 c++의 문법을 사용하여 커스텀(?) 하는 것이다.

```c++
#include <iostream>
#include <queue>

using namespace std;

int main() {
    // 최솟값을 우선순위로
    priority_queue<int, vector<int>, greater<int>> pq;

    pq.push(1);
    pq.push(3);
    pq.push(2);

    cout << pq.top(); // 1

    return 0;
}
```

<br>

## ✏️ 백준

---

기본적인 힙 문법을 활용할 수 있는 간단한 문제들을 풀어보았다.

- [백준 11279 - 최대 힙](https://www.acmicpc.net/problem/11279)
- [백준 1927 - 최소 힙](https://www.acmicpc.net/problem/1927)

응용 문제

- [백준 11286 - 절대값 힙](https://www.acmicpc.net/problem/11286)
- [백준 2075 - N번째 큰 수](https://www.acmicpc.net/problem/2075)
