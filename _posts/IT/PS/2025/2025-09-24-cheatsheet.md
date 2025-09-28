---
title: "알고리즘, 코테용 C++ 치트 시트"
date: 2025-09-24 18:10:00 +0900
last_modified_at: 2025-09-24 18:10:00 +0900
categories: [Algorithm, Problem Solving]  
tags: [cpp, interview, cheatsheet, tips]    
toc: true
comment: false
published: true
image:
    path: "https://www.shutterstock.com/image-illustration/cheatsheet-text-on-blue-grungy-260nw-1808793118.jpg"
---

코테 전날 훑어볼 치트시트

---
## 1. C++ PS 기본 환경설정(Basic Setup)
- 문제 풀이를 시작하기 전 거의 항상 필요한 코드와 팁 
### 1-1.  C++ 필수 코드 템플릿
- 자주 쓰는 헤더 정리
	- `iostream`, `algorithm`, `cmath`, `string`, `bitset`
- 빠른 입출력: `ios_base::sync_with_stdio(false); cin.tie(NULL);`
- 타입 별칭: using ll = long long;, using pii = pair<int, int>;
* endl 대신 '\n' 사용하기
### 1-2 .  로컬 디버깅 템플릿

- 코테용 TC 디버깅 템플릿

```cpp
#include <iostream>

using namespace std;

void solve() {
	int A, B;
	cin >> A >> B;
	cout << A * B;
}

int main() {
	std::ios_base::sync_with_stdio(false);
	std::cin.tie(NULL); std::cout.tie(NULL);

#ifdef _DEBUG
	// 로컬 디버깅 환경에서만 실행하는 코드
	freopen_s(new FILE*, "input.txt", "r", stdin);
	freopen_s("output.txt", "w", stdout);
	int TC;
	cin >> TC;
	for(int i=1; i<=TC; i++) {
		std::cout << "=== Case #" << i << " ===\n";
		solve();
		std::cout << "\n===============\n\n";
	}
#else
	// 채점 서버 환경일때 실행하는 코드. 대부분 단일 TC임
	solve();
#endif

	return 0;
}
```
---
## 2. 문제 접근법 
- 문제를 받고 코딩하기 전까지의 사고 과정 정리
### 2-1. 복잡도 계산
- 시간 복잡도
- N 크기에 따른 허용 시간 복잡도 (1초, 1억 연산 기준)

```
  *   N ≤ 1,000,000: O(N), O(N log N)
  *   N ≤ 10,000: O(N²)
  *   N ≤ 500: O(N³)
  *   N ≤ 20: O(2^N) (조합, 백트래킹)
  *   N ≤ 12: O(N!) (순열)
```
- 공간 복잡도

```
*   int: 4 bytes
*   long long: 8 bytes
*   double: 8 bytes
*   char: 1 byte
  
* int 100만 개 배열: 1,000,000 × 4 bytes = 4,000,000 bytes = 4MB
* int arr[1000][1000]: 1,000 × 1,000 × 4 bytes = 4MB  
  
1KB = 1024byte
4,000,000 Bytes ≈ 4,000 KB ≈ 4 MB
```
- N이 1000 이상일 때, O(N^2) 공간 복잡도를 가진 풀이가 메모리 초과가 발생하는지 꼭 확인할 것
- 특히 재귀는 호출할 때 마다 Call stack에 메모리를 사용하므로, 주의
### 2-2. 자료형
- 정수 범위 체크 습관화
- int 범위: 약 ±2.1e9. `2.1 * 10^9`초과하면 long long 쓸것!!!
- long long 범위: 약 ±9e18
- `int`와 `long long` 혼합 연산 → 자동 승격 안 되므로 주의
- 형 변환에 주의할 것
- 상수 정의

```cpp
const int INF = 21e8; 
const long long LINF = 1e18;
```
### 2-3. 기타
- 수학적 공식을 사용할 수 있을지 확인해보기
	- 합 공식 : `n*(n+1)/2`
	- 제곱 합 : `n(n+1)(2n+1)/6`
	- 등비수열의 합 : `(a(r^n-1))/(r-1)`
	- 순열 개수 : `n!/(n-r)!`
	- 조합 개수 : `n!/(r! * (n-r)!)`
- 모듈러 연산 주의 : `(a-b+MOD)%MOD`
- 빠른 거듭제곱 (fast power, O(logN))
- 전위표기법 (+AB), 중위 표기법 (A+B), 후위표기법(AB+)
---
## 3. Tools

알고리즘 구현에 필요한 C++의 툴들을 정리
### 3-1. STL 컨테이너
- `vector`: 동적 배열. `v.push_back(), v.pop_back(), v.size(), v.clear()`
	- reserve와 resize의 차이 알아둘 것
	- `reserve(n)` : 앞으로 사용할 메모리 공간 미리 예약
		- size는 바뀌지 않음, capacity가 최소 n으로 변경
		- 비효율적인 재할당을 방지하기 위한 메모리 공간을 미리 확보하는 최적화 방법
	- `resize(n)` : 컨테이너의 크기 변경
		- size가 n으로 바뀜, capacity는 필요시 n 이상으로 변경
- `queue`: FIFO. `q.push(), q.pop(), q.front(), q.empty()`
- `stack`: LIFO. `s.push(), s.pop(), s.top(), s.empty()`
- `priority_queue`: 힙. `pq.push(), pq.pop(), pq.top()`
- Min Heap: `priority_queue<int, vector<int>, greater<int>> pq;`

Key-Value 값을 저장하고자 하면 Map
- `map`* `Key-Value, 자동 정렬(Red-Black Tree). O(log N)`
- `unordered_map`: Key-Value, `정렬X(Hash). 평균 O(1)`

원소만 저장하고자 하면 Set
- `set`: 중복 없는 원소, 자동 정렬. O(log N)
- `unordered_set`: 중복 없는 원소, 정렬X. 평균 O(1)
### 3-2. STL 알고리즘 
- sort(v.begin(), v.end()): 정렬
- binary_search(v.begin(), v.end(), val): 값 존재 여부
- lower_bound(v.begin(), v.end(), val): val 이상의 첫 위치 iterator
- upper_bound(v.begin(), v.end(), val): val 초과의 첫 위치 iterator
- next_permutation(v.begin(), v.end()): 다음 순열 생성
- min/max_element
### 3-3. 기타 스킬들

특정 원소 삭제

```cpp
v.erase(remove(v.begin(), v.end(), x), v.end());
// erase 함수는 연속적 특성을 가진 컨테이너에서는 삭제 한 뒤, 값들이 일제히 당겨짐.
// 이때 오버헤드가 발생할 수 있음
// remove는 조건에 맞는 값들을 그 뒤의 값들로 덮어씀
// 이때 쓰레기값이 그대로 남음
// 이 두가지 특성을 이용하여 remove로 선행 값들을 제거한 뒤, 뒤의 중복값들을 erase로 날려버림

// c++ 20에 들어서는 erase, erase_if 함수가 단일 함수 자체로 erase remove idiom을 구현
std::erase(v, 2);
std::erase_if(vec, [](int num){return num < 0;});
```

중복 원소 삭제 ([[unique 함수]] 참조)

```cpp
v.erase(unique(v.begin(), v.end()), v.end()); // unique의 리턴값은 정렬되고 난 후 쓰레기값의 첫번째 위치를 리턴함
```

정렬 커스텀

```cpp
sort(v.begin(), v.end(), [](auto &a, auto &b){ // 람다 함수 사용
    return a.second < b.second;
});
```

원소 개수 세기 : `upper_bound - lower_bound`

---
## 4. 핵심 알고리즘 

본격적인 문제 해결 로직들을 유형별로 정리
### 구현, 시뮬레이션
- 버퍼를 써야하는가? 동시성 문제가 발생하는지 체크할 것
- "동시에" 무언가가 변하는지를 찾아볼 것. 순차성 문제는 버퍼 불필요
[민트 초코 우유](https://www.codetree.ai/ko/frequent-problems/samsung-sw/problems/mint-choco-milk/description)에서는 오히려 동시성 처리를 하면 잘못된 결과가 발생했음

### 완전 탐색

개념
- 모든 경우의 수를 탐색하는 기본 전략 (재귀, next_permutation)
- DFS를 기반으로 하지만, 가지치기(Pruning)을 통해 불필요한 탐색을 줄여 효율을 높힘
- 문제의 모든 후보 해를 나타내는 '상태 공간 트리'를 탐색하는 과정으로 볼 수 있음

예시 문제
- [N과 M 시리즈](https://jinhg0214.github.io/posts/N_and_M/) : 순열, 조합, 중복 허용 순열, 중복 허용 조합의 대표적 문제
- N-Queen


의사 코드

```cpp
## 📌 백트래킹 기본 알고리즘
1. 현재 상태(state)를 확인
2. 만약 조건을 위배하면 → **즉시 리턴(가지치기)**
3. 조건을 만족하면 → 정답 후보인지 확인
4. 다음 단계로 이동 (재귀 호출)
5. 모든 경우를 시도해보고, 끝나면 원상복구(undo)
   
void backtrack(state) {
    if (조건 위배) return;        // 가지치기
    if (정답 조건 만족) { ... }   // 해 발견

    for (선택 가능한 경우들) {
        선택;
        backtrack(업데이트된 state); //idx+1이나 sum+a 
        선택 취소; // 원상 복구 (undo)
    }
}

// vector를 값으로 복사하지 않고, 참조(&)로 전달하는게 효율적
// 재귀호출 직전에 원소를 추가하고, 재귀 호출 이후에 원소를 제거하여 복구하는 방법 사용
void DFS(int level, ..., vector<int>& selected) { 
	 // Base Case (종료 조건)
	 if (level == M) { / 결과 처리 / return; }

	 for (int i = next; i < N; i++) {
		 selected.push_back(i);      // 1. 상태 변경 (선택)
		 DFS(level + 1, ..., selected);
		 selected.pop_back();       // 2. 상태 복구 (선택 해제)
	 }
 }
```

순열, 조합, 중복순열, 중복 조합 의사코드 

```cpp
// 1. 순열 (Permutation)
// arr: 전체 원소 배열, result: 현재까지 만든 순열, visited: 원소 사용 여부 체크
function permutation(result):
	// M개를 모두 뽑았으면 출력
	if length(result) == M:
		print result
		return
	
	// 0번부터 N-1번 원소까지 모두 확인
	for i from 0 to N-1:
		// 아직 사용하지 않은 원소라면
		if visited[i] == false:
			visited[i] = true          // 사용했다고 표시
			add arr[i] to result       // 결과에 추가
			permutation(result)        // 재귀 호출
			remove last from result    // (백트래킹) 원상 복구
			visited[i] = false         // (백트래킹) 원상 복구

// 2. 조합 (Combination)
// start: 탐색을 시작할 인덱스
function combination(result, start):
	// M개를 모두 뽑았으면 출력
	if length(result) == M:
		print result
		return
	
	// start 인덱스부터 N-1번 원소까지 확인
	for i from start to N-1:
		add arr[i] to result
		combination(result, i + 1)  // ★ 핵심: 다음 탐색은 i+1부터 시작
		remove last from result

// 3. 중복순열 (Permutation with Repetition)
function permutation_with_repetition(result):
	// M개를 모두 뽑았으면 출력
	if length(result) == M:
		print result
		return

// 0번부터 N-1번 원소까지 모두 확인 (제약 없음)
for i from 0 to N-1:
	add arr[i] to result // 중복 체크가 빠짐
	permutation_with_repetition(result)
	remove last from result

// 4. 중복조합 (Combination with Repetition)
function combination_with_repetition(result, start):
	// M개를 모두 뽑았으면 출력
	if length(result) == M:
		print result
		return
	
	// start 인덱스부터 N-1번 원소까지 확인
	for i from start to N-1:
		add arr[i] to result // 별도의 중복 체크 불필요. 탐색에서 처리함
		combination_with_repetition(result, i)  // ★ 핵심: 다음 탐색을 i부터 시작
		remove last from result
```

### 그리디 (Greedy)
개념
- 매 순간 가장 좋아보이는 선택을 반복하는 방법
	- '탐욕적 선택이 항상 최적해로 이어진다'는 정당성을 반드시 증명해야 함
	- 직관만으로 사용하면 틀리기 쉬우므로 주의할 것
- 문제를 해결하기 위한 최적의 정렬 기준 찾기
- 정형화된 절차 없음

예시 문제
- [백준 회의실 배정 문제](https://www.acmicpc.net/problem/1931)
* 최소 신장 트리 (MST): 크루스칼(Kruskal), 프림(Prim) 알고리즘
* 다익스트라(Dijkstra) 알고리즘

의사 코드

```cpp
function greedy_solver(inputs):
	// 1. 문제를 해결할 '정렬 기준'을 찾는다. (가장 중요)
	sort(inputs) by the greedy criterion
	
	solution = {}
	
	// 2. 정렬된 순서대로 순회한다.
	for item in inputs:
	  // 3. 현재 아이템을 선택해도 되는지 '적합성'을 검사한다.
	  if is_feasible(item, solution):
		  // 4. 해답에 추가하고 상태를 갱신한다.
		  add item to solution
	
	return solution
```

### 분할 정복 (Divide & Conquer) 
개념
- 큰 문제를 더 이상 나눌 수 없는 작은 문제로 나눈뒤(Divide),  작은 문제들을 해결하고 그 결과를 합쳐 원래 문제의 답을 구하는 전략
- 일반적으로 재귀함수로 구현됨

예시 문제
- 병합 정렬(Merge Sort) : 배열을 반으로 나누고, 각 부분을 정렬한 뒤, 정렬된 두 부분을 다시 합침
- 퀵 정렬(Quick Sort) : 피벗(Pivot)을 기준으로 배열을 두 부분으로 나눈뒤, 각 부분을 재귀적으로 정렬
- 이진 탐색(Binary Search) : 탐색 범위를 절반씩 줄여나가며 답을 찾음

의사 코드

```
function divide_and_conquer(problem):
	// Base Case: 문제가 충분히 작으면 직접 푼다.
	if problem is small_enough:
	  return solve_directly(problem)
	
	// 1. 분할 (Divide)
	subproblems = divide(problem)
	
	// 2. 정복 (Conquer)
	sub_solutions = []
	for sub in subproblems:
	  sub_solutions.append(divide_and_conquer(sub))
	
	// 3. 결합 (Combine)
	result = combine(sub_solutions)
	
	return result
```

### 이분 탐색
개념 
- 정렬된 데이터에서 특정 값을 O(logN)의 시간 복잡도로 빠르게 찾는 알고리즘
- 탐색 범위를 계속해서 절반으로 줄여나가며 답을 찾음
- 주요 활용 분야 '탐색' 혹은 
	- '파라메트릭 서치(Parametric Search)' : 특정 조건을 만족하는 최대/최소값을 찾는 최적화 문제. 이 값이 조건에 맞는가를 반복해서 확인하는 결정 문제로 바꾸어 해결
- 인덱스 값에 주의할것. +1, -1

예시 문제
- [수 찾기](https://www.acmicpc.net/problem/1920) (정렬된 배열에서 숫자 존재 여부 확인)
- [나무 자르기](https://www.acmicpc.net/problem/2805) (원하는 나무 길이를 얻기 위한 절단기 높이의 최댓값 찾기)
- [공유기 설치](https://www.acmicpc.net/problem/2110) (가장 인접한 두 공유기 사이의 거리를 최대로 하기)

의사 코드

```cpp
// 주로 while문을 이용한 반복 형태로 구현
// 특정 조건을 만족하는 최소값을 찾는 파라메트릭 서치 템플릿
int low = 시작값, high = 끝값;
int ans = -1; // 문제에 따라 초기값 설정

while (low <= high) {
	int mid = low + (high - low) / 2;
	
	// mid가 조건을 만족하는가?
	if (check(mid)) {
	  ans = mid;          // 현재 mid는 정답 후보가 됨
	  high = mid - 1;     // 더 좋은 답(더 작은 값)이 있는지 왼쪽 범위 탐색
	} 
	else {
	  low = mid + 1;      // 조건을 만족하지 못하므로 오른쪽 범위 탐색
	}
}
// 최종 ans가 정답
```

### 투 포인터 / 슬라이딩 윈도우 
개념
- 1차원 배열 위에서 두 개의 포인터를 움직여가며, 문제의 조건에 만족하는 특정 구간을 O(N)의 시간 복잡도로 찾는 알고리즘
- 포인터들이 배열의 시작과 끝에서 서로를 향해 움직이거나, 같은 방향으로 이동하며 구간(윈도우)의 크기를 조절함

예시 문제
- [부분합](https://www.acmicpc.net/problem/1806) (합이 S 이상인 가장 짧은 연속 부분 수열 찾기)
- [두 용액](https://www.acmicpc.net/problem/2470) (두 용액을 합쳐 0에 가장 가까운 용액 만들기. 포인터가 양 끝에서 시작)
- [보석 쇼핑](https://programmers.co.kr/learn/courses/30/lessons/67258) (모든 종류의 보석을 포함하는 가장 짧은 구간 찾기)

의사 코드

```cpp
int start = 0, end = 0;
// 현재 윈도우의 상태를 저장할 변수 (e.g., 구간 합, 원소 개수 등)
auto current_state;

while (end < N) {
	// 1. end 포인터를 이동시켜 윈도우를 확장
	window.add(arr[end]);
	
	// 2. 윈도우가 조건을 만족하는 동안, start를 이동시켜 윈도우 축소
	while (window.satisfies_condition()) {
	  // 정답 갱신 (e.g., 최소 길이, 최대값 등)
	  update_answer();
	
	  window.remove(arr[start]);
	  start++;
	}
	
	end++;
}
```

### 유니온 파인드 
개념
- 여러 노드들이 어떤 그룹에 속해있는지 표현하고, 두 노드를 같은 그룹으로 합치는(Union)연산과 특정 노드가 어떤 그룹에 속해있는지 찾는(Find) 연산을 효율적으로 수행하는 자료구조
- '서로소 집합(Disjoing Set)' 자료 구조라고도 부름

예시 문제
- [집합의 표현](https://www.acmicpc.net/problem/1717) (Union, Find 연산 구현)
- [여행 가자](https://www.acmicpc.net/problem/1976) (도시들이 모두 연결되어 여행 계획이 가능한지 판별)
- 최소 신장 트리(MST)를 구하는 크루스칼 알고리즘*에서 사이클 생성 여부 판별

의사 코드

```cpp
// 경로 압축(Path Compression)과 크기 기반 합치기(Union by Size) 최적화가 적용된 C++ 템플릿
// parent[i]: i의 부모 노드
// sz[i]: i가 루트인 경우, 해당 트리의 크기
vector<int> parent(n + 1), sz(n + 1, 1);

// iota: parent 배열을 0, 1, 2, ... , n으로 초기화
iota(parent.begin(), parent.end(), 0);

// find: x의 루트 노드를 찾음 (경로 압축 최적화 포함). 재귀를 사용함
function<int(int)> find = & (int x) {
  if (parent[x] == x) return x;
  return parent[x] = find(parent[x]);
};

// unite: a와 b가 속한 두 그룹을 합침 (크기 기반 최적화 포함)
auto unite = & (int a, int b) {
  a = find(a);
  b = find(b);
  if (a == b) return false; // 이미 같은 그룹

  // 더 작은 트리를 더 큰 트리 밑으로 합침
  if (sz[a] < sz[b]) swap(a, b);
  parent[b] = a;
  sz[a] += sz[b];
  return true;
};
```

### 동적 계획법(DP)
개념
- 복잡한 문제를 여러 개의 작은 겹치는 부분 문제로 나눈뒤, 각 부분의 문제의 답을 한번만 계산하고 그 결과를 저장(캐싱)하여 재활용함으로써 속도를 향상시키는 전략
- 규칙을 찾아내서 점화식을 세우는 것이 가장 중요하지만 어려움

예시 문제
- LIS (Longest Increasing Subsequence)
- [Knapsack (배낭 문제)](https://jinhg0214.github.io/posts/knapsack/)
- [LCS (최장 공통 부분 수열)](https://jinhg0214.github.io/posts/subsequence/) 

의사 코드

```cpp
// 1. 탑다운 (Top-Down) / 메모이제이션 (Memoization)
// dp_table: 계산 결과를 저장하는 배열, -1 등으로 초기화
int dp_table[N];

int solve(state) {
	// Base Case (재귀의 종료 조건)
	if (is_base_case(state)) return base_value;
	
	// 이미 계산한 문제라면 즉시 반환 (Memoization)
	if (dp_table[state] != -1) return dp_table[state];
	
	// 재귀적으로 다음 상태를 호출하여 결과 계산
	int result = /* 점화식에 따른 계산 */;
	
	// 결과를 저장하고 반환
	return dp_table[state] = result;
}
// 2. 바텀업 (Bottom-Up) / 타뷸레이션 (Tabulation)
// dp_table: 계산 결과를 저장하는 배열
int dp_table[N];

// Base Case (가장 작은 문제) 초기화
dp_table[0] = base_value;

// 반복문을 통해 테이블을 채워나감
for (int i = 1; i < N; i++) {
  // 점화식: 이전 결과들(dp_table[i-1] 등)을 이용해 현재 답을 구함
  dp_table[i] = /* 점화식에 따른 계산 */;
}

// 최종 결과 반환
return dp_ta
```

### 4-3. 그래프
#### 표현법
- 인접 행렬
	- 2차원 배열로 표현
	- `adj[u][v] = 1` : u에서 v로 가는 간선 존재. 무방향 그래프면 대칭
	- 두 정점 u,v간 연결 여부 확인이 O(1)
	- 구현이 단순
	- 메모리 사용량이 많음. 간선이 적은 그래프에서 비효율적
- 인접 리스트
	- 배열(벡터) + 연결 리스트 구조
	- 메모리 사용량이 적음 : O(N+E)
	- 희소 그래프에서 효율적
	- 두 정점 u,v 연결 여부 확인은 O(deg(u))
#### 탐색 
DFS, BFS (스택/큐 사용, visited 처리 시점).

BFS 너비 우선 탐색

- `int direct[4][2] = { 0,1, 1,0, 0,-1, -1,0 }`를 이용하여 4방향 순회

```cpp
queue<int> q;
q.push(start);
visited[start] = true;

while (!q.empty()) {
  int curr = q.front(); 
  q.pop();
  for (int next : adj[curr]) {
	  if (!visited[next]) {
		  visited[next] = true;
		  q.push(next);
	  }
  }
}
```

DFS 깊이 우선 탐색
- 그래프/트리에서 모든 노드를 방문하기 위한 탐색 기법
- 백트래킹은 DFS를 기반으로 하지만, 이 경로는 답이 아니다 싶으면 바로 가지치기함

```cpp
void dfs(int curr) {
  visited[curr] = true;
  for (int next : adj[curr]) {
	  if (!visited[next]) {
		  dfs(next);
	  }
  }
}
```

## 최단 경로

### 다익스트라(Dijkstra)

개념
- 그래프의 하나의 시작 정점으로부터 다른 모든 정점으로까지의 최단 경로를 찾는 알고리즘
- 거리 배열은 long long 으로 선언하는 것을 고려할 것
- 우선 순위 큐에서 꺼낸 정보가 최신인지 확인할 것

예시 문제
- [BOJ 1753 - 최단경로](https://www.acmicpc.net/problem/1753)
- [BOJ 1916 - 최소비용 구하기](https://www.acmicpc.net/problem/1916)
- [BOJ 1238 - 파티](https://www.acmicpc.net/problem/1238)

의사 코드

```cpp
// edge에는 edge[start] = {cost, end} 순으로 기록되어 있어야 함

void dijkstra(int start) {
	dist.resize(V + 1);
	for (int i = 0; i < dist.size(); i++) {
		dist[i] = 21e8; 
	}

	priority_queue< pair<int, int>, vector<pair<int,int>>, greater<pair<int,int>>> pq; // C++ 우선순위큐는 최소힙이므로 greater로 정렬해야 역순으로 출력됨
	// 혹은 값을 음수화 하고 최소힙을 사용해도 됨
	pq.push({ 0, start });
	dist[start] = 0;

	while (!pq.empty()) {
		int cur_dis = pq.top().first; // 현재 거리의 코스트
		int cur = pq.top().second; // 현재 노드
		pq.pop();

		for (int i = 0; i < edge[cur].size(); i++) {
			int next_dis = edge[cur][i].first; // 다음 목적지까지의 코스트
			int next = edge[cur][i].second; // 다음 목적지 

			// dist 배열 갱신 부분. 만약 현재 거리 + 다음 목적지 거리가 더 짧으면 갱신
			if (cur_dis + next_dis < dist[next]) {
				dist[next] = cur_dis + next_dis;
				pq.push({ dist[next], next }); // 큐에 넣어준다
			}
		}


	}
}
```

### 0-1 BFS
개념
- 간선의 가중치가 0 또는 1인 그래프에서 최단 경로를 찾는 알고리즘
- 일반적인 BFS와 다익스트라 알고리즘의 특징을 결합한 형태
- 덱을 사용하여, 
	- 덱의 앞에 0-가중치 간선으로 연결된 정점을 추가함으로써, 거리가 증가하지 않는 경로를 우선적으로 탐색한다. 
	- 반대로 1-가중치 간선으로 연결된 정점은 뒤에 추가하여, 현재 거리의 노드를 모두 방문한 뒤에 탐색하여 우선순위를 떨어트린다
- push, pop 연산이 O(1)이므로 O(V+E) 선형 시간 복잡도를 가진다

예시 문제
- BOJ 13549 - 숨바꼭질 3
- BOJ 1261 - 알고스팟

의사 코드

```text
while dq is not empty:
    current_node = dq.pop_front()

    for neighbor, weight in neighbors of current_node:
      // 더 짧은 경로를 발견한 경우
      if dist[current_node] + weight < dist[neighbor]:
        dist[neighbor] = dist[current_node] + weight

		// 가중치 0이라면 앞에, 1이라면 뒤에 넣는다
        if weight == 0:
          dq.push_front(neighbor)
        else: // weight == 1
          dq.push_back(neighbor)
```

### Flood-Fill
개념
- 특정 지점과 연결된, 같은 속성을 가진 인접한 영역을 새로운 속성으로 채우는 알고리즘
- 주로 2차원 배열이나 그리드 형태의 데이터에서 사용함
- 보통 4방향 탐색을 시도하나, 문제에 따라 6, 8방향, 혹은 3차원 시도하는 경우도 있음
- 경계 처리에 주의할 것

### 벨만-포드(Bellman-Ford)
개념
- 한 정점에서 다른 모든 정점까지의 최단 경로를 찾는 알고리즘
- 다익스트라와 달리 음수 가중치 간선 처리가 가능함
- "완화(Relaxation)"이라고 불리는 과정을 반복한다
	- u에서 v로 가는 가중치 w의 간선 (u,v)에 대해서, 현재 알려진 시작점부터 v까지의거리 `dist[v]`보다 시작점에서 u를 거쳐  v로 가는 거리 `dist[u] + w`가 더 짧다면 이로 갱신한다

### 플로이드-워셜(Floyd-Warshall)
개념
- 모든 정점 쌍 간의 최단 경로를 한번에 구하는 알고리즘
- 벨만포드나 다익스트라가 하나의 시작점에서 모든 정점까지의 최단 거리를 구하는것(Single-source)과 달리, 모든 정점에서 출발하여 다른 모든 정점에 도착하는 최단 경로를 모두 계산함
- DP를 이용함
- 어떤 정점 K를 거쳐 가는 경로와 기존에 알려진 경로를 비교하는 아이디어에 기반함
### 4-4. 기타 주요 알고리즘
정수론 
- 소수 판별(에라토스테네스의 체) 

```cpp
for (int i = 2; i * i <= num; i++) {
	if (grid[i]) {
		for (int k = i * i; k <= num; k += i) {
			grid[k] = false;
		}
	}
```

- GCD/LCM : numeric 헤더 사용

```cpp
int gcd(int a, int b) {
	if (a == 0) return b;
	return gcd(b % a, a);
}

int lcm(int a, int b) {
	return (a * b) / gcd(a, b);
}
```
- 모듈러 연산
	- a가 음수이면 = `a%m + m`

문자열 
- KMP 
	- Knuth-Morris-Pratt알고리즘. 문자열 검색 알고리즘
	- 하나의 긴 문자열 안에서 특정 짧은 문자열이 나타내는 모든 위치를 찾아낸다
	- 불일치가 발생한 지점 이전에, 패턴의 저빔사와 접두사가 일치했는지 기억하고, 그 정보를 이용해 패턴을 한칸 이상 점프시킨다
	- 라이브러리에서 제공하는 문자열 찾기 기능의 알고리즘은 보통 KMP 알고리즘보다 느리다
- 트라이(Trie)
	- 문자열을 저장하고 검색하는데 특화된 트리 형태 자료 구조
	- 검색(Retrieval) 이라는 단어에서 유래함
	- 매우 빠른 문자 검색
- [[문자열 파싱]]

최소 신장 트리 (Minimum Spanning Tree ,MST)
- 주어진 가중치가 있는 무방향 그래프에서, 
  모든 정점을 연결하면서 간선들의 가중치의 합이 최소가 되는 트리
	- 신장(Spanning) : 그래프의 모든 정점이 포함되어 연결되어야 함
		- V개의 정점이 있다면, MST는 항상 V-1개의 간선을 가짐
	- 즉, 이 정점들을 모두 연결하면서, 간선들이 합이 최소가 되는 트리
- 크루스칼(Kruskal) 알고리즘
	- 가장 가중치가 낮은 간선부터 차례로 선택하는 알고리즘
- 프림(Prim)
	- 하나의 정점에서 시작해서, 최소 비용의 간선을 계속해서 연결하는 방식 

위상 정렬 (Topological Sort)


구조체 비교

비트마스킹 : 비트 연산을 이용한 집합 표현 및 상태 압축

```cpp
for (int s=0; s<(1<<n); s++) { // 부분집합 순회
    if (s & (1<<i)) { ... } // i번째 원소 포함 여부
}
```

---
## 5. 입출력 
- 다양한 상황에 맞게 입출력 사용하는법
- `cin/cout` 속도 향상 : `ios_base::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)`
### 5-1. 입력 
- 테스트 케이스 개수가 주어지지 않은 경우 : scanf는 입력받는데 성공한 인자들의 수 리턴

```C++
while(cout >> a >> b){ // cout도 EOF에 도달하면 false를 리턴함
	cout << a + b << '\n'; 
}

while(scanf("%d%d", &a, &b)!=EOF)
	printf("%d\n", a+b);
```
- 띄어쓰기를 포함한 줄 입력

```cpp
cin >> N;
cin.ignore(); 
cin.getline(cin, str); 
// 이전에 cin을 사용헀다면 '\n' 가 남아있는 경우가 있으므로 cin.ignore를 써줘야함
```
- 띄어쓰기가 아닌 규칙 입력

```cpp
scanf("%d, %d",&a, &b); // scanf를 사용
```
- getline으로 받은 문자열을 쪼개기

```cpp
#include <sstream>

string line, token;
getline(cin, line);
// getline(cin, line, ',');

stringstream ss(line); // stringstream에 읽은 줄을 넣기
while(ss >> token){ // 공백을 기준으로 자동 파시
	cout << token << "\n";
}
```
- 공백 없이 붙은 문자/숫자 그리드 입력받기

```cpp
  // 4x5 크기의 격자가 공백 없이 주어질 때
  // 11011
  // 01010
  // 10001
  // 11110

  int N = 4, M = 5;
  vector<string> board(N);
  for (int i = 0; i < N; ++i) {
      cin >> board[i]; // 한 줄을 통째로 string으로 읽음
  }

  // 접근: board[i][j] 로 char에 바로 접근 가능
  // char c = board[0][2]; // '0'
  // 숫자로 사용해야 한다면 board[i][j] - '0' 트릭을 사용
```
- 파일 입출력으로 로컬 테스트하기

```cpp
freopen("input.txt", "r", stdin);
freopen("output.txt", "w", stdout);
```
- 테스트 케이스가 주어진 경우, 사용한 메모리들을 꼭 초기화 해야 함
- `endl` 대신 `'\n'` 사용하기
### 5-2. 출력
- cout 출력 포멧팅

```cpp
// 1. 소수점 정밀도 제어
double pi = 3.1415926535;
cout << fixed << setprecision(3) << pi << endl;
// 출력: 3.142
// fixed, setprecision은 한번 설정 시 계속 유지됨 (sticky)

// 2. 출력 폭 지정 및 정렬
cout << left << setw(10) << "hello" << right << setw(10) << "world" << endl;
// 출력: hello          world
// setw(N)는 다음 출력에만 1회 적용됨 (not sticky)
// left, right는 계속 유지됨 (sticky)

// 3. 빈 공간 특정 문자로 채우기
cout << setfill('*') << setw(8) << "hi" << endl;
// 출력: ****hi
// setfill은 한번 설정 시 계속 유지됨 (sticky)

// 4. 진법 변환 출력
int number = 255;
cout << "10진수: " << dec << number << endl; // 255
cout << "16진수: " << hex << number << endl; // ff
cout << "8진수: " << oct << number << endl;  // 377
cout << std::showbase << std::hex << 255 << endl; // 접투두사 표시 0xff
cout << std::showpos << 123 << endl; // 부호 표시
cout << dec; // 다른 출력을 위해 10진수로 다시 돌려놓는 것이 좋음
// dec, hex, oct는 계속 유지됨 (sticky)
```
- printf 출력 포멧팅 : 속도가 빠르고, 형식 문자열 하나로 제어 가능

```cpp
// 0. 기본 자료형 출력
printf("%d %lld %c %s\n", 10, 1234567890LL, 'A', "hello");
// %d: int, %lld: long long, %c: char, %s: C-style string(char*)
std::string s = "world";
printf("%s\n", s.c_str());
// std::string은 .c_str() 멤버 함수를 사용해야 함. 안그러면 깨져서 나옴

// 1. 소수점 정밀도 지정
printf("%.3f\n", 3.141592);
// 출력: 3.142 (반올림됨)

// 2. 출력 폭 지정 및 정렬
printf("[%10s]\n", "hi");
// 출력: [        hi] (기본은 오른쪽 정렬)

printf("[%-10s]\n", "hi");
// 출력: [hi        ] (- 플래그를 붙이면 왼쪽 정렬)

// 3. 빈 공간 0으로 채우기
printf("%05d\n", 42);
// 출력: 00042 (숫자 앞에 0 플래그를 붙임)

// 4. 진법 변환 출력
int number = 255;
printf("10진수: %d\n", number); // 255
printf("16진수: %x\n", number); // ff
printf("8진수: %o\n", number);  // 377
// %d(10진수), %x(16진수), %o(8진수) 지정자 사용
```


---

## 6. 디버깅

제출 전 주요 디버깅 체크리스트 작성 및 적용

- 일반 케이스
- 최소/최대 입력
- 경계 조건
- 예외 케이스
- 자료형 범위
- 시간/공간 복잡도
- 출력 포멧

## 참조

[알고리즘 공부 방법/순서](https://baactree.tistory.com/14)
[복잡도 분석](https://baactree.tistory.com/26)
[문제 해결 전략](https://baactree.tistory.com/27)
[STL 정리](https://baactree.tistory.com/29)

