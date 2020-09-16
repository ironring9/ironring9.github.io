---
title: "연결리스트 Linked List"
categories: 
  - Algorithm
  - Data Structure
toc: true
---



# 연결 리스트

연결 리스트(linked list)는 배열과 마찬가지로 항목의 리스트를 표현하는 자료구조입니다.  

코드에서 배열을 생성하면 아래 그림과 같이 메모리 내에 연속된 빈 공간을 찾아 할당합니다.

![algorithm-linkedList-img001]({{site.url}}/assets/images/algorithm-linkedList-img001.png)

연속된 빈 공간에 저장돼있기 때문에 **배열의 접근은 O(1)의 시간복잡도**를 가집니다.
가령 배열의 메모리 주소가 1000인지 알고 있으며, 인덱스 4를 찾고 싶다면 간단히 메모리 주소 1004로 가면 된다는 것입니다.

이와 달리 연결 리스트는 나란히 이어진 메모리 묶음이 아닙니다. 서로 인접하지 않은 메모리의 묶음으로 이뤄집니다.  
이와 같이 서로 인접하지 않은 메모리를 **노드**라 부릅니다.

**그렇다면 어떻게 컴퓨터는 서로 인접해있지 않은 노드들이 같은 연결 리스트에 속하는지 알 수 있을까요?**

이 답이 바로 연결 리스트의 핵심입니다. 각 노드는 노드에 저장된 데이터뿐만 아니라 연결 리스트 내에 다음 노드의 메모리 주소도 저장합니다.

![algorithm-linkedList-img002]({{site.url}}/assets/images/algorithm-linkedList-img002.png)

위 예제를 보면 연결 리스트에 Data와 **다음 노드의 주소**를 가리키는 값이 있습니다. 실제 노드들의 주소는 각기 다르지만 다음 노드의 주소를 알고 있기 때문에 노드들이 같은 연결 리스트에 속하는지 알 수 있게됩니다.

## Array vs Linked List

### Array

- 메모리에 연속된 공간에 할당된다.
- 인덱스만 알고 있다면 O(1)만에 해당 인덱스에 접근할 수 있다.
- 즉, Random Access가 가능하다.
- 제한된 크기를 갖는다.
- 삽입, 삭제, 데이터 이동이 어렵다.
- Compile time에 메모리 할당이 이뤄진다.(정적 메모리할당)
- Stack 영역에 할당

### Linked List

- 메모리에 연속되지 않은 데이터(노드)들로 구성된다.
- 인덱스 접근은 순차 접근 방식을 사용하여 O(n)의 시간 복잡도를 갖는다.
- 삽입, 삭제, 데이터 이동이 쉽다.
- 새로운 노드가 추가될 때 즉, Run time에 메모리 할당이 이뤄진다. (동적 메모리할당)
- Heap 영역에 할당

# 연결 리스트 구현

자료구조는 보통 C언어로 구현하지만 C언어로는 학부생 때 이미 해봤고 Javascript로 구현해보는것도 나름 재밌을 것 같아 도전해 봅니다.

[LinkedList 소스](https://github.com/ironring9/data_structure_by_js/blob/master/LinkedList.js)

```js
function Node (data) {
    this.data = data;
    this.nextNode = null;
}

function LinkedList () {
    this.length = 0;
    this.head = null;
}
```

Node와 LinkedList는 위와 같이 구현했습니다.

우선 Node는 data와 다음 node를 가리키는 nextNode를 프로퍼티로 갖고있습니다.  
LinkedList는 현재 list의 길이와 시작 node를 가리키는 head를 프로퍼티로 갖습니다.

## 추가
```js
LinkedList.prototype.add = function (data) {
    var node = new Node(data);
    var current = this.head;
    if (!current) {
        this.head = node;
    } else {
        while (current.nextNode) {
            current = current.nextNode;
        }
        current.nextNode = node;
    }

    this.length++;
}
```

- arguments로 넘어온 data를 사용해서 Node 객체를 생성합니다.
- 연결리스트의 head가 null일 경우 비어있다는 의미이므로 this.head에 바로 노드를 추가합니다.
- 연결리스트가 비어있지 않을경우 마지막까지 탐색 후 노드를 추가합니다.

## 삭제
```js
LinkedList.prototype.remove = function (index) {
    var current = this.head;
    var currentIndex = 0;
    if (!current) return;

    if (index == 0) {
        var node = this.head;
        this.head = node.nextNode;
        delete node;
    } else {
        while (crrentIndex < index) {
            current = current.nextNode;
            currentIndex++;
        }

        var node = current.nextNode;
        current.nextNode = node.nextNode;
        delete node;
    }

    this.length--;
}
```

- index가 0일 경우 this.head에 this.head가 가리키는 다음 노드(nextNode)를 가리키게 하고 현재 노드는 제거한다.
- index가 0이 아닐 경우 대상 index 전까지 탐색
- 삭제 대상 노드의 다음노드를 이전노드와 연결한다.
- 삭제 대상 노드 메모리 해제

## 탐색
```js
LinkedList.prototype.get = function (index) {
    var current = this.head;
    var currentIndex = 0;

    while (currentIndex <= index) {
        current = current.nextNode;
        currentIndex++;
    }

    return current.data;
}
```

- head 부터 해당 index까지 순차적으로 탐색한다. 시간 복잡도: O(n) 

## 삽입
```js
LinkedList.prototype.insert = function (data, index) {
    var node = new Node(data);
    var current = this.head;
    var currentIndex = 0;

    if (index == 0) {
        node.nextNode = this.head;
        this.head = node;
    } else {
        while (currentIndex < index) {
            current = current.nextNode;
            currentIndex++;
        }

        node.nextNode = current.nextNode;
        current.nextNode = node;
    }

    this.length++;
}
```

- index가 0일 경우 새로 생성한 노드의 nextNode를 head로 할당하고 head를 새로 생성한 노드로 할당.
- index가 0이 아닐 경우 대상 index 전까지 탐색한다.
- 새로 생성한 노드(node)의 nextNode를 탐색한 노드(current)의 nextNode로 할당한다.
- current의 nextNode를 node로 할당한다.