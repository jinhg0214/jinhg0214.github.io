---
title: "Lower Bound, Upper Bound"
date: 2023-12-13 17:07:00 +0900
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [search, binarysearch, lowerbound, upperbound]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

# Lower Bound, Upper Bound 란?

---

- [Binary Search](https://jinhg0214.github.io/posts/binary_search/)의 응용버전
	- 이진 탐색은 정렬된 데이터에서 특정 값이 존재하는지 검색하는 알고리즘
	- 그러나 **중복된 데이터**에서 탐색할 때는 사용 볼가!!
- 이때 사용하는것이 Lower Bound, Upper Bound임 
	- 이진탐색 -상한선, 하한선으로도 불림

![img](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/48715bf9-d2c7-40c2-a73c-90ae4c865705)

- Lower Bound는 특정 값 K보다 같거나 큰 값(K<=X)이 처음 나오는 위치를 리턴
	- 위 사진에서 Lower_bound(3)을 호출하면, 3보다 같거나 큰값은 index = 3이므로 3을 리턴함
- Upper Bound는 특정 값 K보다 처음으로 큰 값(K<X)이 나오는 위치를 리턴
	- Upper_bound(3)을 호출하면, 처음으로 3보다 큰값이 나오는 위치인 index = 6을 리턴함
- 이분 탐색을 기반으로 하므로, 시간 복잡도는 O(logN)

# 예시

---

![img1 daumcdn](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/787a0e59-36b1-49e4-af79-3c6ff4e9f7cb)

- 3번 그림에서 5가 없는데 lower_Bound(5)를 호출하면,   

  5보다 같거나 큰 6을 찾고 그 index인 7이 호출된다.   
  
  Upper_bound(5)도 마찬가지로 5보다 큰 값인 6을 찾아 7이 리턴
	
    - 즉, upper건 lower건 일치하는 값이 없다면 그보다 큰 값을 찾고, 그 인덱스를 리턴함

- 4번 그림은 8보다 같거나 큰 수나, 8보다 큰수는 배열에 존재하지 않으므로 배열의 끝을 가리키게됨

- 5번 그림은 0보다 같거나 큰 수는 1, 0보다 큰수는 1이므로 index = 0 이 리턴된다 

# 구현
---
- 알고리즘 문제에서는 이분탐색을 응용하는 문제들이 출제되므로 직접 구현하여 수정할 줄 알아야함
- lower bound 와 upper bound는 기본 이분 탐색 구현에서 살짝 수정되는 부분만 주의할 것

### lower bound
- 주어진 값보다 같거나 큰 값이 처음으로 나오는 곳을 리턴해야하므로, 
  존재하지 않는 큰 수가 있을 때 `end = array.length-1`로 하면 
  배열의 마지막 인덱스를 넘겨주게 된다. 따라서 `end = array.length`로 해야한다
- 기본 이진탐색과 다른점은 값을 찾아도 바로 리턴하는것이 아니라, 
  처음으로 나오는 값을 찾기 위해 범위를 좁혀가며 찾는다.
  그래서 end = mid로 하고 범위를 좁혀나가는 것 
```cpp
int lower_bound(vector<int> v, int target) {
	int start = 0;
	int end = v.size();
	int mid;
	while (start < end) {  // 주의점 1. start <= end 등호가 없다
		mid = (start + end) / 2;
		
		// 목표값이 중간값보다 작거나 같은 경우
		if (target <= v[mid]) { // 주의점 2. <= 등호가 있음. 바로 리턴하는것아니라 범위를 좁히는게 목적
			end = mid; // 주의점 3. mid + 1이 아닌 mid 임
		}
		// 목표값이 더 큰 경우, 시작값을 끌어올린다
		else {
			start = mid + 1;
		}
	}
    // 주의점 4. true, flase 값이 아닌 인덱스를 반환함
	return start; // 시작값을 리턴하면 처음으로 같거나 큰 수가 나오는 인덱스를 반환
}
```

### Upper Bound 
- lower bound와 마찬가지로 upper bound는 주어진 값보다 큰 값이 처음으로 나오는 걸 리턴해야함. 존재하지 않는 큰 수가 있을 것을 대비해 `end = array.length`로 지정함
- lower bound와 달리, 탐색한 값이 주어진 값보다 크다면, 바로 리턴하지 않고, 최초로 큰 값이 있는 위치를 찾기 위해 `end = mid` 로 지정하여 범위를 줄여나간다

```cpp
int upper_bound(vector<int> v, int target) {
	int start = 0;
	int end = v.size();
	int mid;

	while (start < end) {
		mid = (start + end) / 2;
		
		// 목표값이 더 크거나 같은 경우
		if (target >= v[mid]) { // 등호가 이쪽에 들어갔다
			start = mid + 1;
		}
		else {
			end = mid;
		}
	}
	return start; // 마찬가지로 시작 지점을 리턴함
}
```

# STL
--- 
- C++은 `algorithm` 헤더에 정의되어있음

```cpp
// 참고로 C++ 20 부터 모두 constexpr 함수 이다.
template <class ForwardIt, class T>
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value);
ForwardIt upper_bound(ForwardIt first, ForwardIt last, const T& value);
```
- lower_bound
	- 범위 `[first, last)`안의 원소들 중, value 보다 같거나 큰 값의 첫번째 원소의 주소를 리턴함 
	- 원소가 존재하지 않는다면 `last`를 리턴함
	- 주소를 리턴하므로, 인덱스에 접근하려면 `lower_bound(arr, arr+N, val) - arr` 처럼 
	  시작 주소를 빼줘야 인덱스를 얻을 수 있다
- upper_bound
	- 범위 `[first, last)`안의 원소들 중,  value 보다 큰 첫번째 원소를 리턴함
	- 원소가 존재하지 않는다면 last를 리턴함

```cpp
template <class ForwardIt, class T, class Compare>
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value,
                      Compare comp);
ForwardIt upper_bound(ForwardIt first, ForwardIt last, const T& value,
                      Compare comp);
```
- 구조체 정렬하기과 유사하게 comp 함수를 직접 구현하여 정렬 기준을 정할 수 있음
- 4번째 매개변수로 함수를 넘겨주면 됨