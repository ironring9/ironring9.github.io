---
title: "[Algorithm] 삽입 정렬(Insertion Sort)"
categories: 
  - Algorithm
  - Sort
toc: true
---

# 삽입정렬이란?

이번에는 삽입정렬에서 알아보겠다.

삽입정렬은 다음과 같은 단계를 따른다.

1. 기준 요소(첫 번째 셀)의 오른쪽에 있는 값을 선택한다.
2. 기준 요소의 값과 비교해서 크면 오른쪽에 작으면 왼쪽의 자기자리에 삽입한다.
3. 리스트가 모두 정렬될 때 까지 1,2 단계를 반복한다.

![algorithm-insertionSort-img001]({{site.url}}/assets/images/algorithm-insertionSort-img001.png)

**선택정렬의 시간복잡도는 O(n^2)로 성능은 좋은 편이 아니다.**

# 구현

[삽입정렬 소스](https://github.com/ironring9/data_structure_by_js/blob/master/InsertionSort.js)
