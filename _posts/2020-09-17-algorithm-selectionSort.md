---
title: "[Algorithm] 선택 정렬(Selection Sort)"
categories: 
  - Algorithm
  - Sort
toc: true
---

# 선택정렬이란?

선택정렬은 가장 이해하기 쉬운 정렬 방법이다.

선택정렬은 다음과 같은 단계를 따른다.

1. 리스트의 각 요소를 왼쪽부터 오른쪽 방향으로 확인하면서 최솟값을 찾는다.
2. 최솟값을 찾으면 현재 기준이 되는 요소와 교환한다.
3. 리스트가 모두 정렬될 때 까지 1,2 단계를 반복한다.

![algorithm-selectionSort-img001]({{site.url}}/assets/images/algorithm-selectionSort-img001.png)

**선택정렬의 시간복잡도는 O(n^2)로 버블정렬과 동일하지만 실제로는 두 배 더 빠르다고한다.**

# 구현

[선택 소스](https://github.com/ironring9/data_structure_by_js/blob/master/SelectionSort.js)
