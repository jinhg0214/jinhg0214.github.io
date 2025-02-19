---
title: "정렬 안정성 Sort Stablity"
date: 2024-11-13 14:46:00 +0900
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [sort, stable, unstable]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://i.ytimg.com/vi/ClG4xjwQ0BM/maxresdefault.jpg"
    alt: 
---

## 1. Stablity
---

>A sorting method is _stable_ if it preserves the relative order of equal keys in the array.
>정렬 후에도 같은 키들의 상대적인 순서가 정렬 이전과 동일하게 유지되는 정렬 방법을 안정 정렬이라고 부른다.

- 정렬의 안정성이란, 키-값 쌍을 가진 객체들 중 **같은 키를 가진 객체들의 순서가 정렬 이후에도 유지되는 성질**
- 이를 **유지하는 정렬을 Stable Sort,** **유지하지 못하는 정렬을 Unstable Sort**

### 왜 중요한가?
---

- Stable Sort로 정렬하는 알고리즘들의 순서는 항상 같기 때문에, 결과가 항상 같음을 보장한다
- 숫자를 정렬할때는 안정성이 크게 중요하지 않음
- 그러나 첫 문자를 기준으로 문자열을 정렬할때는 Stable 하지 않다면, 결과값이 계속 바뀌게됨

## 2. Stable Sort
---

- **안정성(Stablity)**을 보장하는 정렬
- **중복된 키를 순서대로 정렬하는 정렬 알고리즘**
	- 같은 값이 2개 이상 리스트에 등장할 때, 기존 리스트에 있던 순서대로 중복된 키들이 정렬됨
- 반대로 순서가 유지되지 않는 [[Unstable Sort]]

### 대표적인 Stable Sort
- Insertion Sort (삽입 정렬)
- Bubble Sort (버블 정렬)
- Merge Sort (병합 정렬)
- Heap Sort (힙 정렬)
- Quick Sort (퀵 정렬)
- Selection Sort (선택 정렬)
- 알고리즘의 세부 구현 방식에 따라 달라질 수 있음

## 3. Unstable Sort
---

- [[정렬 안정성]]와 반대로, 정렬 후 기존의 순서가 유지되지 않는 정렬
- **중복된 값들의 순서가 변할 수 있는 알고리즘**
	- 같은 값이 2개 이상 리스트에 등장할 때, 기존 리스트에 있던 순서대로 중복된 키들이 정렬되지 않음
- C++ algorithm의 `sort`는 unstable sort이므로, 정렬성이 필요하다면 `stable_sort`를 사용해야한다

### 대표적인 Unstable Sort

- Counting Sort (계수 정렬)
- Radix Sort (기수 정렬)
- 알고리즘의 세부 구현 방식에 따라 달라질 수 있음

## 4. 예시
---

첫번째 키를 기준으로 정렬하는 경우

- Stable Sort의 경우 

```cpp
입력: [(1, 'a'), (3, 'b'), (1, 'c'), (2, 'd')]
Stable Sort 수행
출력: [(1, 'a'), (1, 'c'), (2, 'd'), (3, 'b')]
```

`(1, 'a'), (1, 'c')`처럼 원래 순서를 유지하면서 정렬된다

몇번을 수행하더라도 같은 결과가 보장된다

- Unstable Sort의 경우, 

```
입력: [(1, 'a'), (3, 'b'), (1, 'c'), (2, 'd')]
Unstable Sort 수행
출력: [(1, 'c'), (1, 'a'), (2, 'd'), (3, 'b')]
```

첫번째 키 값은 정렬이 보장되지만,

동일한 키값`(1, 'a), (1,'c')` 에 대해서는 원래의 순서가 보장되지 않는다

이는 내부 동작에 따라 달라질 수 있음

같은 데이터라도 실행 환경이나 정렬 방식 또는 내부 구현에 따라 결과가 달라질 수 있다

내가 사용하는 정렬이 어떤 안정성을 갖고 있는지 잘 알아야한다
