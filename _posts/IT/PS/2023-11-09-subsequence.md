---
title: "최장 공통 부분 수열(Longest Common Subsequence)"
# excerpt : 요약
date: 2023-11-09 00:04:23 +0900
# last_modified_at: 
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [lcs, dp, string, subsequence]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

최장 공통 문자열(Longest Common Substring) 과는 다른

최장 공통 부분 수열(Longest Common Subsequence)

LCS의 길이를 구하는 방법과 LCS문자열을 구하는 방법 두가지를 알아본다

# 최장 공통 부분 수열이란?
--- 

Substring이 연속된 문자열이라면

Subsequence는 꼭 연속된것은 아닌, 부분 문자열이다. 그러나 순서가 뒤바뀌는것은 불가능하다

예들을어 문자열 ABCDEF가 주어진다면 부분 수열은 

A, B , .. , F, AB, AC, AD ,... , ... ACDF, ABCDEF 등이 있다

ACB는 순서가 뒤바뀌므로, 부분 수열이 아니다

# 1. 백준 9251 LCS 길이 구하기
---

https://www.acmicpc.net/problem/9251

### 간단 설명

문자열 두개가 주어질 때, 최대 공통 부분 수열(LCS)의 길이를 구하기

## 문제 분석
---
### 알고리즘
LCS를 구하는 방법은 Longest Common Substring을 구하는 방법과 유사하다

대신 몇가지가 더 추가된다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/e3e186ea-fac7-4719-87c0-cf184a479f31)

y=0, x=0인 부분은 0으로 채운다 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/0293b2fe-6481-47a0-9551-9c4acb72e814)

두 문자가 일치하는 칸은 `lcs[y-1][x-1]` 값에 +1 을 해서 가져온다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/994982a3-c094-485f-82c2-6e9f29626173)

두 문자가 일치하지 않는 칸은 `lcs[y][x-1]`과 `lcs[y-1][x]`의 값을 비교한뒤, 큰 값을 가져온다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/9c7005bd-bf2c-4b41-82e8-7f58df121c5a)

이런식으로 표를 끝까지 채우면, 우측하단이 LCS의 최대 길이가 된다

### 필요변수
문자열 `str1 str2`

`lcs 테이블`

최대값 저장용 `int max_cnt`

### 주의점
- 배열의 인덱스 참조에 주의
- 첫번째 줄은 0으로 처리하므로, `lcs[1][1]`부터 값이 들어가 된다
- 또한 행과 열을 다룰 때 각 문자열이 행인지 열인지 잘 체크해야한다

## 소스코드
---
```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

#define MAX 1001
int lcs[MAX][MAX];

int main() {
	freopen_s(new FILE*, "input.txt", "r", stdin);
	string str1, str2;
	cin >> str1 >> str2;

	int n = str1.size();
	int m = str2.size();

	int max_cnt = 0;
	for (int y = 0; y <= m; y++) {
		for (int x = 0; x <= n; x++) {
			if (y == 0 || x == 0) {
				lcs[y][x] = 0;
			}
			else if (str1[x-1] == str2[y-1]) {
				lcs[y][x] = lcs[y - 1][x - 1] + 1;
			}
			else {
				lcs[y][x] = max(lcs[y - 1][x], lcs[y][x - 1]);
			}
			max_cnt = max(max_cnt, lcs[y][x]);
		}
	}
	cout << max_cnt;

	return 0;
}
```

# 2. 백준 9252 LCS 2 출력하기

https://www.acmicpc.net/problem/9252

### 간단 설명

문자열 2개가 주어질 때, LCS의 길이와 그 내용을 출력하기

## 문제 분석
---
### 알고리즘

- LCS 테이블을 채우는 과정을 역으로 추적한다

```
1. 배열의 마지막 값에서부터 왼쪽과 위쪽으로 올라가본다
2. `lcs[i-1][j]`와 `lcs[i][j-1]`중 현재값과 같은 값을 찾는다
	같은 값이 있다면 해당 값으로 이동
	같은 값이 없다면 result 배열에 해당 문자를 넣고 `lcs[i-1][j-1]` 로 이동
3. 0 으로 이동하게 되면 종료
4. result 배열을 뒤집어서 출력한다
```
- 2번에서 우선순위를 왼쪽에 주느냐, 오른쪽에 주느냐에 따라 출력되는 값이 달라진다

### 소스코드
```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

#define MAX 1001
int lcs[MAX][MAX];

int main() {
	freopen_s(new FILE*, "input.txt", "r", stdin);
	string str1, str2;
	cin >> str1 >> str2;

	int n = str1.size();
	int m = str2.size();

    // lcs의 길이를 구하면서 테이블을 채우는 과정 
	int max_cnt = 0;
	for (int y = 0; y <= m; y++) {
		for (int x = 0; x <= n; x++) {
			if (y == 0 || x == 0) {
				lcs[y][x] = 0;
			}
			else if (str1[x - 1] == str2[y - 1]) {
				lcs[y][x] = lcs[y - 1][x - 1] + 1;
			}
			else {
				lcs[y][x] = max(lcs[y - 1][x], lcs[y][x - 1]);
			}
			max_cnt = max(max_cnt, lcs[y][x]);
		}
	}

    // lcs의 문자열을 구하는 과정
	string result;

    // 오른쪽 끝에서 시작한다
	int yy = m;
	int xx = n;

	while (lcs[yy][xx] != 0) {
        // 위쪽에 우선순위를 줌
		if (lcs[yy][xx] == lcs[yy - 1][xx]) {
			yy--;
		}
		else if (lcs[yy][xx] == lcs[yy][xx - 1]) {
			xx--;
		}
		else {
			result += str2[yy-1];
			yy--;
			xx--;
		}
	}
	reverse(result.begin(), result.end());
	cout << max_cnt << endl;
	cout << result << endl;

	return 0;
}
```

참조 : 

https://www.geeksforgeeks.org/longest-common-substring-dp-29/

https://velog.io/@emplam27/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-LCS-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Longest-Common-Substring%EC%99%80-Longest-Common-Subsequence