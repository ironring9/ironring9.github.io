---
title: "[Algorithm] 버블 정렬(Bubble Sort)"
categories: 
  - Algorithm
  - Sort
toc: true
---

# 버블정렬이란?

버블 정렬은 인접한 두 수를 비교하여 정렬이 되지 않았다면 서로 교환하는 정렬 알고리즘이다.  
왼쪽에서 오른쪽으로 비교&교환을 거치면서 가장 큰 수가 맨 오른쪽에 위치하게 된다.

![algorithm-bubbleSort-img001]({{site.url}}/assets/images/algorithm-bubbleSort-img001.png)

**버블정렬의 시간복잡도는 O(n^2)로 성능은 좋은 편이 아니다.**

# 구현

[버블정렬 소스](https://github.com/ironring9/data_structure_by_js/blob/master/BubbleSort.js)