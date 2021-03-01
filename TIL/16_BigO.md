---
layout: post
title: Big-O 표기법 쉽게 이해하기
subtitle: "Big O notation"
type: "Algorithm"
createDate: 2021-03-01
til: true
text: true
author: "codeFabian"
post-header: false
header-img: ""
order: 16
---

# Big-O 표기법 정리

<br>

알고리즘의 시간복잡도는 입력 값의 증가 또는 감소와 관련된 런타임을 의미합니다.

**Big-O** 란, 알고리즘의 성능과 복잡성을 설명하는데 사용하는 표기법입니다.

어떠한 알고리즘을 실행하는데 필요한 **실행시간**(시간복잡성) 과 **공간** (공간복잡성) 을 계산하고, **해당 알고리즘이 효율적인지, 유용한지를 판단할 수 있습니다.**

알고리즘의 런타임을 고려할 때 **최악, 평균 및 최상**의 시나리오가 있으며 **3가지 모두 서로 다른 시간 복잡도**를 가지고 있습니다.

> 일반적으로는 최악의 시나리오를 상정했을 때의 복잡도에 중요성을 가지고 있다고 합니다.

<br>

## 일반적인 Big-O 표기법 예시

---

![Untitled](https://user-images.githubusercontent.com/46562138/109467138-14fdaf00-7aae-11eb-8059-04c042df67fa.png)

(다음 사진은 일반적인 Big-O 표기법의 각 시간복잡도에 대한 그래프입니다.)

<br>

### O(1) - Constant Complexity

---

입력 값에 관계없이 동일한 시간이 걸리면 항상 일정한 시간복잡도를 갖습니다.

즉, 알고리즘의 시간복잡도가 일정하다면, 입력값이 크기에 관계없이 항상 동일한 시간에 실행됩니다.

가장 많이 알려져있는 예시로는 **배열에서 요소의 위치를 알고 있다면, 그 요소를 찾는데 걸리는 시간은 항상 같습니다.**

```jsx
const num = [1, 2, 3, 4, 5];
const str = ["a", "b", "c", "d", "e"];
// num[0],str[0]
// 입력 된 값에 관계없이 우리는 배열의 첫번째 요소를 찾을 때, 항상 같은 시간이 걸립니다.
```

![Untitled 1](https://user-images.githubusercontent.com/46562138/109467167-21820780-7aae-11eb-83ae-51106fc4afd8.png)

<br>

### O(n) - Linear Complexity

---

선형복잡도는 입력된 값이 증가함에 따라서 시간복잡도가 선형적으로 증가하는 알고리즘 입니다.

**즉, 입력된 데이터의 양이 증가하면 증가할수록 시간복잡도도 일정하게 증가하는 것을 의미합니다.**

예시로는 반복문을 생각할 수 있습니다. 데이터의 수가 5개인 배열과 100개의 배열을 반복문을 통해서 조회한다면, 시간적인 차이를 보일 것 입니다.

![Untitled 2](https://user-images.githubusercontent.com/46562138/109467177-247cf800-7aae-11eb-861d-607f074f8b32.png)

<br>

### O(n^2) - Quadratic Complexity

---

입력된 값의 제곱에 런타임이 정비례하는 알고리즘 입니다.

**즉, 데이터의 양에 따라 걸리는 시간이 제곱으로 늘어나며, 효율이 좋지않은 알고리즘 입니다.**

예시로는, 입력된 데이터가 들어있는 배열을 반복하고 현재 조회한 요소를 다른 모든요소와 비교하는 것을 생각 할 수 있습니다. ex) 이중 반복문

> n개의 요소를 반복해야하고, 모든요소에 대해 다시 n개의 요소를 반복한다면, n\*n 즉 n^2가 될 것 입니다

![Untitled 3](https://user-images.githubusercontent.com/46562138/109467189-2941ac00-7aae-11eb-9217-1634afa97181.png)

<br>

### O (log(n)) - Logarithmic Coplexity

---

**입력크기가 반복될 때마다 시간복잡도가 절반으로 나뉘어지는 알고리즘**입니다.

즉, 입력크기가 100일 때 런타임이 1초가 걸리는 함수가 있다고 가정한다면, 입력크기가 1,000일때는 2초, 10,000 일때는 3초가 걸리게 됩니다.

대표적인 예시로는 **이진탐색트리** 알고리즘이 있습니다.

![Untitled 4](https://user-images.githubusercontent.com/46562138/109467202-2cd53300-7aae-11eb-95bd-d49e33244753.png)

<br>

### O (2^n) - Exponential Complexity

---

입력 값이 추가 될 때마다 런타임이 두 배가 되는 알고리즘 입니다.

1개의 데이터를 처리하는데 30초가 걸린다고 가정하면, 2개의 데이터를 처리할때는 60초, 3개의 데이터를 처리할 때는 90초가 걸리게 되는 **극히 비효율적인 알고리즘입니다**

주로 문제를 해결하기 위해 모든조합과 방법을 시도해야할 때 사용됩니다.

![Untitled 5](https://user-images.githubusercontent.com/46562138/109467206-2f378d00-7aae-11eb-8ea0-51491c7da70d.png)

<br>

---

사진출처: https://www.jenniferbland.com/time-complexity-analysis-in-javascript/
