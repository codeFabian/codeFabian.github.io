---
layout: post
title: DataStructure Stack & Queue
subtitle: "dataStructure"
type: "dataStructure"
createDate: 2021-03-02
til: true
text: true
author: "codeFabian"
post-header: false
header-img: ""
order: 17
---

# What is Stack

---

**스택은 특정한 순서로만 저장(삽입)하거나 삭제할 수 있는 자료구조 입니다.**

일상생활에서의 스택을 예시로 장독대를 생각해보겠습니다.

장독대에 김치를 넣는다고 생각해보겠습니다. 처음에 넣은 김치는 가장 아래에 있기 때문에 마지막에 꺼낼 수 있을 것 입니다. 반대로 마지막에 넣은 김치는 가장 위에 있기 때문에 가장 먼저 꺼낼 수 있게 됩니다.

**마지막에 넣은 데이터를 가장 먼저! 처음에 넣은 데이터를 가장 마지막에 뽑아낼 수 있는 스택의 가장 중요한 개념인 LIFO, "Last In First Out" 입니다.**

![Untitled](https://user-images.githubusercontent.com/46562138/109604217-7d0fcc00-7b66-11eb-97d9-173ae79964db.png)

(열악한 제 그림입니다^^)

<br>

## Implementation Stack

---

스택에서 필요한 method 로는 `push` 와 `pop` 이 있습니다.

가장 먼저 데이터를 저정할 공간인 Stack을 만들어봅니다.

```jsx
class Stack {
  constructor() {
    this.stack = [];
  }
}
```

그리고 데이터를 삽입 및 저장하기 위해서 `push` 를 사용합니다.

```jsx
class Stack {
	constructor() {
		this.stack = [];
	}
	// 데이터 삽입
	push(data) {
		this.stack.push(data);
	}
}

// 데이터가 잘 들어갔는지 확인할 필요가 있겠죠?
let checkStack = new Stack();

checkStack.push(1)
checkStack.push(2)

console.log(checkStack)
--> Stack {
  stack: [ 1, 2 ],
  __proto__: Stack { constructor: ƒ Stack(), push: ƒ push() }
}
```

이번에는 데이터를 제거해보도록 하겠습니다. 우리는 데이터를 제거할 것이지만 인자로 특정 데이터를 지정할 필요가 없습니다. 왜냐하면 스택에서는 **LIFO** 로 인하여 마지막 데이터가 제거되야하기 깨문이죠

```jsx
class Stack {
	constructor() {
		this.stack = [];
	}
	// 데이터 삽입
	push(data) {
		this.stack.push(data);
	}
	// 삭제한 데이터를 리턴
	pop() {
		return this.stack.pop();
	}
}

// 데이터가 잘 삭제 되어지는지 확인을 해보겠습니다.
let checkStack = new Stack();
checkStack.push(1)
checkStack.push(2)
// 위에서 checkStack 에 데이터 1과 2를 넣었습니다.
checkStack.pop()
// 삭제!

console.log(checkStack)
-->
2 // 삭제한 데이터를 리턴했기 때문에 삭제 된 2가 출력되었습니다.
Stack {
  stack: [ 1 ],
  __proto__: Stack { constructor: ƒ Stack(), push: ƒ push(), pop: ƒ pop() }
}

```

마지막으로 스택에 들어가있는 데이터를 출력해보도록 하겠습니다. 위의 예시와는 조금 다른 부분이 있습니다

```jsx
class Stack {
	constructor() {
		this.stack = [];
	}
	// 데이터 삽입
	push(data) {
		this.stack.push(data);
	}
	// 삭제한 데이터를 리턴
	pop() {
		return this.stack.pop();
	}
	viewStack() {
		let data = "";
		for(let i = 0; i< this.stack.length; i++) {
			data += this.stack[i];
		}
		return data;
	}
}

let checkStack = new Stack();

checkStack.push('h')
checkStack.push('e')
checkStack.push('l')
checkStack.push('l')
checkStack.push('o')
checkStack.viewStack()

console.log(checkStack)
-->

'hello'

Stack {
  stack: [ 'h', 'e', 'l', 'l', 'o' ],
  __proto__: Stack {
    constructor: ƒ Stack(),
    push: ƒ push(),
    pop: ƒ pop(),
    viewStack: ƒ viewStack()
  }
}
```

<br>

## Stack TimeComplexity

---

**특정 값을 얻거나, 검색할 때의 시간복잡도**

- 평균: O(n)
- 최악: O(n)

스택 안의 특정한 요소에 대한 값을 얻기위해서는 스택안의 요소들을 조회해야합니다. 따라서 위의 시간복잡도가 요구됩니다.

**삽입 및 삭제**

- 평균: O(1)
- 최악: O(1)

삽입 및 삭제를 할 때, 가장 맨위에서 데이터를 삽입하거나 삭제하기 때문에 어떠한 요소를 넣는다 하더라도 위의 시간복잡도가 요구됩니다.

**공간복잡성**

: 스택안의 데이터가 늘어날수록 용량도 정비례로 늘어나기 때문에 O(n)이 요구되어집니다.

<br>

# What is Queue

---

큐는 선입선출 원칙을 따르는 자료구조 입니다.

즉, 먼저 들어온 데이터가 가장 먼저 나가는 구조인 것 입니다.

이것을 우리는 **FIFO, "First In First Out"** 이라고 부릅니다.

일상생활에서의 예시로는, 수강신청, 영화예매를 예로 들 수 있겠습니다. 먼저 예매한 자리가 먼저 빠지는 구조 입니다. ( 리그오브레전드라는 게임에서 게임대기열의 이름을 "큐" 라고 불려지는데 이 부분도 이러한 느낌일듯한 ..ㅎㅎ)

![Untitled 1](https://user-images.githubusercontent.com/46562138/109604254-8ef16f00-7b66-11eb-91e1-7f26c3233032.png)

(네... 제가그린그림입니다 ..)

<br>

## Implementation Queue

---

큐에서 필요한 method는 `push` 와 `shift` 입니다.

먼저 데이터를 저장할 배열 Queue의 생성합니다.

```jsx
class Queue {
  constructor() {
    this.queue = [];
  }
}
```

그리고 노드를 큐에 추가시켜줍니다. 우리는 큐에서 데이터를 삽입시키는 것을 **enqueue**라고 부릅니다.

```jsx
class Queue {
  constructor() {
    this.queue = [];
  }
  enqueue(data) {
    this.queue.push(data)
  }
}

// Queue에 데이터가 잘 삽입되었는지 확인을 해보겠습니다.
let checkQueue = new Queue
checkQueue.enqueue(1)
checkQueue.enqueue(2)
checkQueue.enqueue(3)

console.log(checkQueue)
-->

Queue {
  queue: [ 1, 2, 3 ],
  __proto__: Queue { constructor: ƒ Queue(), enqueue: ƒ enqueue() }
}
```

이번에는 큐에 추가시켜준 데이터를 다시 삭제시켜보겠습니다.

우리는 큐에서 삭제시키는 것을 **dequeue** 라고 부릅니다.

```jsx
class Queue {
  constructor() {
    this.queue = [];
  }
  enqueue(data) {
    this.queue.push(data)
  }
	dequeue() {
		this.queue.shift();
	}
}

let checkQueue = new Queue
checkQueue.enqueue(1)
checkQueue.enqueue(2)
checkQueue.enqueue(3)
// Queue에 데이터가 잘 삭제 되는지 확인을 해보겠습니다.
checkQueue.dequeue()
checkQueue.dequeue()

console.log(checkQueue)
-->
Queue {
  queue: [ 3 ],
  __proto__: Queue {
    constructor: ƒ Queue(),
    enqueue: ƒ enqueue(),
    dequeue: ƒ dequeue()
  }
}
```

<br>

## Queue TimeComplexity

---

**특정 값을 얻거나, 검색할 때의 시간복잡도**

- 평균: O(n)
- 최악: O(n)

특정 값을 얻거나 조회를 할 때 우리는 큐의 모든 요소들에 접근을 해야합니다. 따라서 시간복잡도는 O(n) 으로 큐의 요소들의 수에 정비례하게 됩니다.

**삽입 및 삭제**

- 삽입: O(1)
- 삭제: O(n)

삽입을 할 때는, 새로운 데이터를 큐의 끝으로 푸쉬하게 됩니다. 따라서 시간복잡도는 항상 같은 O(1)이 됩니다.

하지만 데이터를 삭제하고 싶을 땐, 전체 배열을 조회하여 마지막 항목을 반환하기 때문에 시간복잡도는 큐의 요소들에 정비례하는 O(n)이 되게 됩니다.

**공간복잡성**

: 큐의 데이터가 늘어날수록 용량도 정비례로 늘어나기 때문에 O(n)이 요구되어집니다.
