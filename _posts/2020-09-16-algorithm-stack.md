---
title: "스택 Stack"
categories: 
    - Algorithm
    - Data Structure
toc: true
---

# 스택

스택은 다음과 같은 특징을 갖습니다.  

- 데이터는 스택의 끝에만 삽입할 수 있다.
- 데이터는 스택의 끝에서만 읽을 수 있다.
- 데이터는 스택의 끝에서만 삭제할 수 있다.

이 특징들을 한마디로 표현하면 **후입선출(Last In First Out)**이라 할수 있습니다.  
즉, 마지막으로 입력한 것이 첫 번째로 나오는 것이죠.  

실생활에서 후입선출을 이용한 것을 예로 들어보면 종이컵 수거함이나 차곡차곡 쌓여진 책을 예로 들수 있습니다.  
프로그래밍에서는 정적할당에 사용하는 메모리 스택영역이나 함수 콜스택 등을 예로 들수 있습니다.

![algorithm-stack-img001]({{site.url}}/assets/images/algorithm-stack-img001.png)

# 스택 구현

[Stack 소스](https://github.com/ironring9/data_structure_by_js/blob/master/Stack.js)

## 노드 구성

```js
function Node (data) {
    this.data = data;
    this.next = null;
}

function Stack () {
    this.count = 0;
    this.top = null;
}
```

- Node는 자신의 data와 다음 노드를 가리킬 next를 갖습니다.
- Stack은 자료의 갯수(count)와 마지막으로 들어온 노드를 가리키는 top을 갖습니다.

## 추가 (Push)

```js
Stack.prototype.push = function (data) {
    var node = new Node(data);
    node.next = this.top;
    this.top = node;
    return ++this.count;
}
```

- 새로 생성한 노드의 다음 노드는 현재의 top이 됩니다.
- Stack은 마지막으로 들어온 노드만 참조하면 되기 때문에 top을 현재 노드로 바꿉니다.
- 이렇게 하면 후입, 즉 마지막으로 들어온 노드를 바로 접근할 수 있게 됩니다.

## 삭제 (Pop)

```js
Stack.prototype.pop = function () {
    if (this.count == 0) return null;
    
    var node = this.top;
    var data = node.data;
    this.top = node.next;
    this.count--;
    return data;
}
```

- Pop은 Stack이 가리키고있는(top) 노드를 꺼내고 그 아래 노드를 가리키게 하는 동작입니다.
- Stack은 마지막으로 들어온 노드를 바로 알수 있기 때문에 O(1)의 시간복잡도를 갖습니다.