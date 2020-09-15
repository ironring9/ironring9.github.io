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

사실 자료구조를 Javascript로 구현하는 것도 좀 웃기긴 하지만 나름 재밌을 것 같아 도전해 봅니다.

[LinkedList 소스](https://github.com/ironring9/data_structure_by_js/blob/master/LinkedList.js)