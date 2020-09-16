---
title: "큐 Queue"
categories: 
    - Algorithm
    - Data Structure
toc: true
---

# 큐

큐는 다음과 같은 특징을 갖습니다.

- 데이터는 큐의 끝에만 삽입할 수 있다.
- 데이터는 큐의 앞에서만 읽을 수 있다.
- 데이터는 큐의 앞에서만 삭제할 수 있다.

이 특징들을 한마디로 표현하면 **선입선출(First In First Out)**이라 할수 있습니다.  
즉, 처음 입력한 것이 첫 번째로 나오는 것이죠.  

실생활에서 선입선출을 이용한 것을 예로 들어보면 사람들이 서있는 줄을 예로 들 수 있습니다.  
컴퓨터로는 프린트, 비동기식 요청을 예로 들 수 있습니다.

# 큐 구현

[Queue 소스](https://github.com/ironring9/data_structure_by_js/blob/master/Queue.js)

## 노드 구성

```js
function Node (data) {
    this.data = data;
    this.next = null;
}

function Queue () {
    this.count = 0;
    this.front = null;
    this.back = null;
}
```

- 큐는 선입선출, 즉 입구와 출구가 다릅니다. 때문에 front(출구)와 back(입구)를 갖습니다.

## 추가 (Enqueue)
```js
Queue.prototype.enqueue = function (data) {
    var node = new Node(data);

    if (!this.front) {
        this.front = node;
    } else {
        this.back.next = node;
    }

    this.back = node;
    return ++this.count;
}
```

- back은 항상 마지막으로 들어온 노드를 가리키기 때문에 enqueue에서 새로 생성한 노드를 this.back에 할당합니다.
- 큐가 비어있을 경우 front는 새로 생성한 노드를 가리킵니다.
- 큐가 비어있지 않을 경우 큐의 끝(back)의 다음노드로 생성한 노드를 가리킵니다.

## 삭제 (Dequeue)
```js
Queue.prototype.dequeue = function () {
    var node = this.front;
    var data = node.data;

    this.front = node.next;

    return data;
}
```

- front가 가리키는 노드를 제거해야하기 때문에 front는 현재의 front의 다음 노드를 가리키게 합니다.