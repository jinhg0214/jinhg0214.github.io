---
title: "Binary Search"
date: 2023-12-06 20:30:00 +0900
# last_modified_at: 
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [binarysearch, cpp, stl, python]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: false
---

# Binary Search란?
---
![binary-and-linear-search-animations](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/89517c3a-bd06-4450-a509-c3aecd7faccf)
- 이분탐색, 또는 이진 탐색이라고도 불림
- **정렬되어있는 리스트**에서 **탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법**
	- 정렬이 안되어있으면 사용 불가능!!
- 변수 3개 (Start, End, Mid)를 사용하여 탐색한다
	- 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교하여 원하는 데이터를 찾음
- 재귀함수로 구현하는 방법과, 반복문으로 구현하는 방법이 있음
- 시간 복잡도는 O(logN)
	- 단계마다 탐색 범위를 반으로 나누는것과 동일

# 알고리즘
---
1. 첫번째 원소를 start, low 등으로 설정
2. 마지막 원소를 end, high 등으로 설정
3. 검사할 인덱스를 mid, center 로 선언 후, (start + end) / 2 의 값으로 ㅅ설정
4. 이 값을 검색 할 값과 비교한다
	- 만약 mid 값이 더 크다면, 검사 범위를 앞쪽으로 줄인다. End를 mid로 설정, Start는 그대로 둠
	- 만약 mid 값이 더 작다면, 검사 범위를 뒷쪽으로 줄인다. Start를 mid로 설정, End는 그대로 둠
	- 일치한다면 인덱스를 리턴
5. start가 end를 넘어간다면 종료한다 (찾는 원소가 없기때문)

# 반복문으로 구현하는 방법
---
```cpp
// 배열의 시작 주소와 길이, 원하는 타겟을 입력받아
// 값이 존재하는지 확인하는 함수
bool binary_search(vector<int>& arr, int len, int target){
	int start = 0;, end = len -1;
	
	while(start <= end){
		int mid = (start + end) / 2;
		// 원하는 값을 찾은 경우
		if(target == arr[mid]) return true;
		// 원하는 값이 더 작다면, 앞쪽을 검사한다
		if(target < arr[mid]){
			end = mid - 1; // -1, +1 을 언제 해줄지 중요함 
		}
		// 원하는 값이 더 크다면, 뒤쪽을 검사한다
		else if(target > arr[mid]){
			start = mid + 1;
		}
	}
	return false; // 마지막까지 못찾은 경우
}
```

# 재귀함수로 구현
---
```cpp
// 배열 시작 주소와
// 시작 인덱스, 종료 인덱스, 타겟을 입력받아
// 값이 존재하는지 확인한다
bool binary_search(vector<int>& arr, int start, int end int target){
	if(start > end) return false;
	int mid = (start + end) / 2;
	
	if(target == arr[mid]) return true;

	if(arr[mid] > target)
		return binary_search(arr, start, mid -1, target);
	else if(arr[mid] < target)
		return binary_search(arr, mid + 1, end, target);
}

```

# C++ STL 라이브러리 이용
---
```cpp
template <class ForwardIt, class T>
bool binary_search(ForwardIt first, ForwardIt last, const T& value);  // (1)

template <class ForwardIt, class T, class Compare>
bool binary_search(ForwardIt first, ForwardIt last, const T& value,
                   Compare comp);  // (2)
```
- 반복자의 시작 지점과 끝점을 입력받고, 찾고자 하는 수를 입력받음

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
  std::vector<int> haystack{1, 3, 4, 5, 9};
  std::vector<int> needles{1, 2, 3};

  for (auto needle : needles) {
    std::cout << "Searching for " << needle << '\n';
    if (std::binary_search(haystack.begin(), haystack.end(), needle)) {
      std::cout << "Found " << needle << '\n';
    } else {
      std::cout << "no dice!\n";
    }
  }
}
```


# Pyhton 라이브러리 이용
---
Python에서는 `bisect`라는 이진 탐색 라이브러리 지원

- `bisect_left(a, x)` : 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 왼쪽 인덱스를 찾는 메소드
- `bisect_right(a, x)` : 정렬된 순서를 유지하도록 리스트 a에 데이터 x를 삽입할 가장 오른쪽 인덱스를 찾는 메소드
```python
from bisect import bisect_left, bisect_right
a = [1, 2, 4, 4, 8]
x = 4

print(bisect_left(a, x))
>>> 2
# 리스트 a에 4를 삽입할 가장 왼쪽 인덱스는 2이다.

print(bisect_right(a, x))
>>> 4
# 리스트 a에 4를 삽입할 가장 오른쪽 인덱스는 4이다.
```