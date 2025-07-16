---
title: "C++ 우선순위 큐"
date: 2024-11-09 22:30:00 +0900
categories: [C++, Library]  
tags: [algorithm, datastructure, queue, sort]     
toc: true
comment: false
published: true
image:
    path: "https://dotnettrickscloud.blob.core.windows.net/article/data%20structures/3720240402230348.com-png-to-webp-converter"
    alt: 
---

알고리즘 문제를 풀다보면, C++에서 우선순위 큐를 사용해야할 경우가 종종 생긴다

이를 기록하기 위해 정리한 글

## 우선순위 큐
- C++에서 vector와 같은 container adaptor의 한 종류
- 큐의 한 종류로 `#include <queue>`에 포함되어있음
- 모든 원소 중에서 가장 큰 값이 Top을 유지하도록 설계된 컨테이너
- 내부적으로는 Heap 자료 구조를 사용하고 있음
	- 각 원소가 우선순위를 가지고 있어, 우선순위를 갖는 원소가 앞쪽에 오게 됨
- 일종의 자동으로 정렬되는 큐라고 생각하면 편함

## 생성자
---
`priority_queue<자료형, 내부 컨테이너, 정렬기준>` 으로 선언
- 자료형 : int, double, 구조체 등등..
- 기본적으로 vector<자료형>으로 정의됨. 벡터 이외의 다른 배열을 사용해도 되나 벡터가 제일 편함
- 알고리즘 문제풀때는 간단히 `priority_queue< pair<int, int> >` 이런식으로 간략히 쓰는데, 실제로는
  `priority_queue< pair<int, int>, vector<pair<int,int>>, less<int> > pq` 로 알아서 만들어 주는것임 
- 정렬 기준 : `less<자료형>()` → Max Heap, `greater<자료형>()` →min heap

## 멤버 함수
---
- `bool empty()` : 비었는지 체크
- `size_type size()` : 크기 체크
- `const value_type & top()` : 맨 앞 원소 체크
- `void push(const value_type& val)` : 원소 넣기. 우선순위에 따라 위치 알아서 조절됨
- `void pop()` : 맨 앞 원소 빼기

## 원하는 순서대로 원소 정렬하기
---

cpp에서 우선순위 큐는 기본적으로 **최대힙**으로 동작한다

이는 큰 수가 가장 먼저 pop되므로, 내림차순으로 출력되는 것을 의미하는데

```cpp
priority_queue<int> pq;
// priority_queue<int, vector<int>, less<int>> pq; 와 같음 // std::less<>이용

pq.push(3);
pq.push(2);
pq.push(7);
pq.push(2);
pq.push(5);

```
최대힙, 내림차순이므로 `pq : 7 5 3 2 2` 로 정렬된다

만약 오름차순으로 출력하고 싶다면

```cpp
priority_queue<int, vector<int, greater<int> > pq; // 선언시 오름차순 정렬로 선언 // std::greater<> 이용

pq.push(3);
pq.push(2);
pq.push(7);
pq.push(2);
pq.push(5);
```

최소힙, 오름차순이므로 `pq : 2 2 3 5 7` 로 정렬된다

### 2. 사용자 정의 비교 함수 구현 

하나의 원소를 비교할 때는 `std::less<>, std::greater<>` 를 써도 충분하지만

두개 이상의 원소를 비교할 때는 직접 비교 함수를 구현해야한다

비교함수를 작성하는 것은 내용이 많아 다음 포스팅으로 나눔

