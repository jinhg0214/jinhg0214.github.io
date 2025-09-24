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

코테가서 볼 한페이짜리 정보 모음

작성중

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
- int 범위: 약 ±2.1e9
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
---
## 3. 도구 상자
- 알고리즘 구현에 필요한 C++의 툴들을 정리
### 3-1. STL 컨테이너
- `vector`: 동적 배열. `v.push_back(), v.pop_back(), v.size(), v.clear()`
- `queue`: FIFO. `q.push(), q.pop(), q.front(), q.empty()`
- `stack`: LIFO. `s.push(), s.pop(), s.top(), s.empty()`
- `priority_queue`: 힙. `pq.push(), pq.pop(), pq.top()`
- Min Heap: `priority_queue<int, vector<int>, greater<int>> pq;`
- `map`* `Key-Value, 자동 정렬(Red-Black Tree). O(log N)`
- `unordered_map`: Key-Value, `정렬X(Hash). 평균 O(1)`
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
- 본격적인 문제 해결 로직들을 유형별로 정리
### 4-1. 기본 알고리즘

완전 탐색 / 백트래킹 : 모든 경우의 수를 탐색하는 기본 전략 (재귀, next_permutation)
- [N과 M 시리즈](https://jinhg0214.github.io/posts/N_and_M/)
- 백트래킹은 DFS를 기반으로 하지만, 불필요한 탐색을 줄이고 정답만 찾는다 
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
        backtrack(업데이트된 state);
        선택 취소; // 원상 복구 (undo)
    }
}
```

그리디 (Greedy) : 정렬 후 선택, 기준점 찾기 등.

분할 정복 (Divide & Conquer)*: 큰 문제를 작은 문제로 나누어 해결.

이분 탐색 : 정렬된 데이터에서 탐색, Parametric Search (최적화 문제).
```cpp
int low = 0, high = N - 1, ans = -1;
while (low <= high) {
  int mid = low + (high - low) / 2;
  if (check(mid)) { // 조건 만족
	  ans = mid;
	  high = mid - 1; // 더 작은 쪽 탐색
  } else {
	  low = mid + 1;
  }
}
```

투 포인터 / 슬라이딩 윈도우*: 1차원 배열에서 특정 조건을 만족하는 구간을 O(N)에 찾는 기술

유니온 파인드
```cpp
vector<int> parent(n+1), sz(n+1,1);
iota(parent.begin(), parent.end(), 0);

function<int(int)> find = [&](int x){
    return parent[x]==x ? x : parent[x]=find(parent[x]);
};

auto unite = [&](int a,int b){
    a=find(a), b=find(b);
    if(a==b) return false;
    if(sz[a]<sz[b]) swap(a,b);
    parent[b]=a; sz[a]+=sz[b];
    return true;
};

```

### 4-2. 동적 계획법(DP)
LIS (Longest Increasing Subsequence)
Knapsack (배낭 문제)
LCS (최장 공통 부분 수열)
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

#### 최단 경로

다익스트라(Dijkstra)

가중치 0/1 -> 0-1 BFS (deque) 활용

Blood-Fill

벨만-포드(Bellman-Ford)

플로이드-워셜(Floyd-Warshall).

### 4-4. 기타 주요 알고리즘
- 정수론 : 소수 판별(에라토스테네스의 체), GCD/LCM, 모듈러 연산
- 문자열 : KMP, 트라이(Trie) 등
- 최소 신장 트리 (MST): 크루스칼(Kruskal), 프림(Prim)
- 위상 정렬 (Topological Sort)
- 구조체 비교
- 비트마스킹 : 비트 연산을 이용한 집합 표현 및 상태 압축
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
while(scanf("%d%d", &a, &b)!=EOF)
	printf("%d\n", a+b);
while(cout >> a >> b){ // cout도 EOF에 도달하면 false를 리턴함
	cout << a + b << '\n'; 
}
```
- 띄어쓰기를 포함한 줄 입력
```cpp
cin.ignore(); cin.getline(cin, str); 
// 이전에 cin을 사용헀다면 '\n' 가 남아있는 경우가 있으므로 cin.ignore를 쓸 것
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
- 공백 없이 이어붙은 문자/숫자 그리드 입력받기
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
// do something
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
// std::string은 .c_str() 멤버 함수를 사용해야 함

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
- 코너 케이스 체크
최소값, 최대값, 음수/0 입력, 중복 원소, 경계 조건
출력 확인

## 참조

[알고리즘 공부 방법/순서](https://baactree.tistory.com/14)
[복잡도 분석](https://baactree.tistory.com/26)
[문제 해결 전략](https://baactree.tistory.com/27)
[STL 정리](https://baactree.tistory.com/29)

