---
title: "우선순위 큐"
date: 2024-11-09 22:30:00 +0900
categories: [C, Library]  
tags: [algorithm, datastructure, queue, sort]     
toc: true
comment: false
published: true
image:
    path: "https://dotnettrickscloud.blob.core.windows.net/article/data%20structures/3720240402230348.com-png-to-webp-converter"
    alt: 
---

알고리즘 문제를 풀다보면, 우선순위 큐를 사용해야할 경우가 종종 생긴다

이를 기록하기 위해 정리한 글

## 우선순위 큐
- 구현시 Heap을 사용함
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

### 1. 원소 오름차순 정렬

cpp에서 우선순위 큐는 기본적으로 최대힙으로 동작한다

이는 큰 수가 가장 먼저 pop되므로, 내림차순으로 출력되는 것을 의미하는데

```cpp
priority_queue<int> pq;
// priority_queue<int, vector<int>, less<int>> pq; 와 같음

pq.push(3);
pq.push(2);
pq.push(7);
pq.push(2);
pq.push(5);

// pq : 7 5 3 2 2 
```
최대힙, 내림차순이므로 다음과 같이 정렬된다

만약 오름차순으로 출력하고 싶다면

```cpp
priority_queue<int, vector<int, greater<int> > pq; // 선언시 오름차순 정렬로 선언

pq.push(3);
pq.push(2);
pq.push(7);
pq.push(2);
pq.push(5);

// pq : 2 2 3 5 7
```

최소힙, 오름차순이므로 다음과 같이 정렬된다

### 2. 사용자 정의 비교 함수 구현 

하나의 원소를 비교할 때는 `less<int>, greater<int>` 를 써도 충분하지만

두개 이상의 원소를 비교할 때는 직접 비교 함수를 구현해야한다

비교함수를 구현하는 것은 구조체 정렬 함수를 선언하는 것과 유사하다

```cpp
bool operator()(int a, int b){
	if(a ? b) return true;
	else return false;
	// return a ? b;
}
```
위 코드에서 오름차순으로 정렬하고 싶을 때 a와 b의 부등호를 어떤 방향으로 넣어야할까?

비교함수에서 언제 `true`를 리턴해야하는지 이해하는게 중요한데,

비교함수에서 리턴값(bool값)은 자리를 바꿀지 말지를 결정한다

**true를 반환하면 자리를 바꾸고, false를 반환하면 바꾸지 않는다**

- 오름차순이라면 작은 값이 앞으로 와야한다
	- 작으면 바꾸지 않는다
	- 즉 a < b일때 자리를 바꾸지 않으므로 `false`를, 그 외에는 `true`리턴

- 내림차순이라면 큰 값이 앞으로 와야한다
	- 작다면 바꿔야한다
	- 즉, a < b일때 자리를 바꿔야 하므로 `true`를, 그 외에는 `false`를 리턴

이를 이용해 하나의 원소를 정렬하는 `less<int>`와 `greater<int>`를 직접 구현해본다

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct my_lesser{
	bool operator()(int a, int b){
		if(a < b) return true;
		else return false;
		// return a < b; 와 같음
	}
}

struct my_greater {
	bool operator()(int a, int b) {
		if(a < b) return false;
		else return true;
		// return a > b; 와 같음
	}
};

int main() {
	// 기본 : 루트가 최대인 우선순위 큐 (최대힙)
	priority_queue<int> q1; 
  // priority_queue<int, vector<int>, my_lesser> q1; // 과 같음
	
	// 루트가 최소값인 우선순위 큐 (최소힙)
	priority_queue<int, vector<int>, greater<int>> q2; 
	
	// 커스텀 함수를 이용한 정렬 우선순위 큐 선언
	priority_queue<int, vector<int>, my_greater> q3; 
	
	// pair 자료형 오름차순 정렬
	priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q4; 
}
```

**비교함수에서 true를 반환하면 우선순위가 낮아 뒤로 간다는것을 명심할 것!!**

### 2. 두개 이상의 값 비교

이를 응용하면 여러값을 정렬하는 우선순위 큐도 사용할 수 있다

```cpp
struct item{
	int val1;
	int val2;
	int val3;
} a, b;

pq.push({1, 5, 10}); 
pq.push({2, 4, 12});
pq.push({1, 6, 8}); 
pq.push({1, 5, 7});

정렬 기준은

1. val1는 작은 순서대로
2. val2는 큰 순서대로
3. val3은 작은 순서대로 
{1, 6, 8}
{1, 5, 7}
{1, 5, 10}
{2, 4, 12}
```
로 저장되게 해야하는 경우 다음과 같이 함수를 구현할 수 있다

```cpp
struct cmp{
	bool operator()(item a, item b){
		if (a.val1 != b.val1) {
		    if (a.val1 > b.val1) return true; 
		    // b가 우선 나와야 한다 -> a의 우선순위 낮음
		    else return false;                
		    // a가 우선
		}
		if (a.val2 != b.val2) {
		    if (a.val2 < b.val2) return true; 
		    else return false;                
		}
		if (a.val3 > b.val3) return true; 
		else return false;                
	}
}
```
**우선순위가 낮을때 true**이므로, 
- val1 비교
	- a = {1, 5, 10}, b = {2, 4, 12} 인 경우
	- `a.val1`와 `b.val1` 비교시 어떤 부등호를 넣어야할까
	- `a.val1 < b.val1` 는 `true`, `a.val1 > b.val1` 는 `false`다
	- 오름차순으로 정렬하고 싶으므로, a가 b보다 우선순위가 낮아야한다 
		- 그러므로 `a.val1 < b.val1`를 넣어야한다
- val2 비교
	- a = {1, 5, 10}, b= {1, 6, 8} 인 경우,
	- `a.val2 = 5` 와 `b.val2 = 5`를 비교
	- `a.val2 < b.val2` 는 true, `a.val2 > b.val2` 는 `fasle` 다
	- 내림차순으로 정렬하고 싶으므로, a가 b보다 우선순위가 높아야한다
		- 그러므로, `a.val2 > b.val2`를 넣어야한다
- 이런식으로 val3도 넣는다

```cpp
struct cmp{
	bool operator()(item a, item b){
		if(a.val1 != b.val1)
			return a.val1 > b.val1;  // val1이 작은 순서로
		if(a.val2 != b.val2) 
			return a.val2 < b.val2; // val2가 큰 순서 
		return a.val3 > b.val3 // val3이 작은 순서
	}
}
```
처럼 구현하면 된다

결과적으론

```
val1: 1, val2: 6, val3: 8 
val1: 1, val2: 5, val3: 7 
val1: 1, val2: 5, val3: 10 
val1: 2, val2: 4, val3: 12
```
로 내부에서 정렬된다

은근 햇갈리므로 잘 기억해둘 것!!
