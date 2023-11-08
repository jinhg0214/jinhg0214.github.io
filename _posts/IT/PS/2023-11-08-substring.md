---
title: "5582 공통 부분 문자열"
# excerpt : 요약
date: 2023-11-08 11:03:23 +0900
# last_modified_at: 
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [string, lcs, dp, substring]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

최장 공통 부분 수열(Longest Common Subsequence) 와는 다른

최장 공통 문자열(Longest Common Substring) 문제

두 문자열 중에서 공통되는 문자열 중 가장 긴 길이를 구하는 문제

# 1. 문제

공통 부분 문자열

https://www.acmicpc.net/problem/5582

---

### 간단 설명

두 문자열 ABRACADABRA와 ECADADABRBCRDARA의 공통 부분 문자열은 

CA, CADA, ADABR, 빈 문자열 등이 있다. 

이 중에서 가장 긴 공통 부분 문자열은 ADABR로 길이는 5

# 2. 문제 분석

---

### 알고리즘
[문자열1][문자열2] 만큼 2차원 배열 생성 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/79228d83-42ca-44cb-9632-2a2302fdca93)


행과 열의 문자가 다르다면 0을, 같다면 map[y-1][x-1] + 1 을 적는다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f79d3b9f-ab16-4292-8829-8377d301569b)

    - E열은 같은 문자가 없어서 모두 0으로 처리됨
    - C열은 6번째 행이 C로 같아 `map[1][4] + 1` 로 1이 저장됨

이런식으로 표를 끝까지 채운다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/34fc3635-518d-41f6-8710-e34139d00623)
_수작업이라 틀린 부분이 있을수도 있음_

표에서 최대값을 찾아 출력한다. 이게 공통 문자열의 최장 길이다 

위 표에서는 `map[9][10] = 5`로 가장 크다

### 필요변수

문자열 `str1, str2`

공통 문자열을 카운팅 할 2차원 배열 `map[MAX][MAX]`

최대값을 저장할 `int max_cnt`

### 주의점

배열의 인덱스 참조에 주의할 것

표를 작성할때는 문자열 길이만큼 2중 for문을 순회해야하지만

답을 찾을때는 문자열 길이들 +1 만큼 순회해야한다

(map[0] 열과 [0] 행은 0이므로)



# 3. 소스코드

---

```cpp
#include <iostream>
#include <string>

using namespace std;

#define MAX 4001
int map[MAX][MAX];

int main() {
	freopen_s(new FILE*, "input.txt", "r", stdin);
	string str1, str2;
	cin >> str1 >> str2;
	
	int m = str1.size();
	int n = str2.size();
	int max_len = 0;

	// 표를 채우면서 동시에 최대값을 찾는다
	for (int y = 0; y <= n; y++) { // str2. 
		for (int x = 0; x <= m; x++) { // str1
			// 첫 열과 행은 0으로 처리해둠
			if (y == 0 || x == 0) {
				map[y][x] = 0;
			}
			// 문자가 같으면 
			else if (str1[x] == str1[y]) {
				map[y][x] = map[y - 1][x - 1] + 1;
				if (max_len < map[y][x]) {
					max_len = map[y][x];
				}
			}
			// 다르면 0 
			else {
				map[y][x] = 0;
			}
		}
	}
	
	cout << max_len;

	return 0;
}
```

참조 : 

https://www.geeksforgeeks.org/longest-common-substring-dp-29/

https://velog.io/@emplam27/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-LCS-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Longest-Common-Substring%EC%99%80-Longest-Common-Subsequence