---
title: "이중 연결 리스트 Double Linked List"
categories: 
    - Algorithm
    - Data Structure
toc: true
---

# 이중 연결 리스트

이중 연결 리스트는 연결 리스트와 달리 노드의 연결이 이전(prev), 다음(next)으로 이루어져 있습니다.  
또한, 이중 연결 리스트는 리스트 앞과 끝 모두에 바로 접근할 수 있어 O(1)에 양 끝에 데이터를 삽입할 수 있을 뿐 아니라 O(1)에 양 끝에서 데이터를 삭제할 수 있습니다.

### 연결리스트와 다른점

연결리스트는 노드의 다음 노드만을 참조하기 때문에 단방향 탐색만 가능합니다.  
이중 연결리스트는 노드의 다음, 이전 노드를 참조하기 때문에 양방향 탐색이 가능합니다.  


![algorithm-doubleLinkedList-img001]({{site.url}}/assets/images/algorithm-doubleLinkedList-img001.png)

# 이중 연결 리스트 구현

[DoubleLinkedList 소스](https://github.com/ironring9/data_structure_by_js/blob/master/DoubleLinkedList.js)

## 추가
```js
DoubleLinkedList.prototype.add = function (data) {
    var node = new Node(data);
    if (!this.tail) {
        this.head = node;
        this.tail = node;
    } else {
        this.tail.nextNode = node;
        node.prevNode = this.tail;
        this.tail = node;
    }

    this.length++;
}
```

- this.tail 즉, 현재 리스트가 비어있을 경우 head와 tail은 노드를 가리킨다.
- 리스트가 비어있지 않을 경우 리스트 끝에 추가하기 때문에 tail과 연결을 해줍니다. 이 때, 이중 연결 리스트이기 때문에 node의 prevNode도 연결해줍니다.

## 삭제
```js
DoubleLinkedList.prototype.remove = function (index) {
    if (!this.head) return;

    if (index == 0) {
        var current = this.head;
        if (current.nextNode) {
            this.head = current.nextNode;
            this.head.prevNode = null;
        } else {
            this.head = null;
            this.tail = null;
        }
            
        delete current;
    } else if (index == this.length - 1) {
        var current = this.tail; 

        this.tail = current.prevNode;
        this.tail.nextNode = null;
        delete current;
    } else {
        var current = this.head;
        var currentIndex = 0;

        while (currentIndex != index) {
            current = current.nextNode;
            currentIndex++;
        }

        current.prevNode.nextNode = current.nextNode;
        current.nextNode.prevNode = current.prevNode;
        delete current;
    }

    this.length--;
}
```

- 이중 연결 리스트는 시작과 끝을 가리키는 head, tail이 있기 때문에 시작과 끝의 삭제는 O(1)의 시간복잡도를 갖습니다.
- 시작, 끝 외의 노드는 탐색을 해야하기 때문에 O(N)의 시간 복잡도를 갖습니다.