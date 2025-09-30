---
title: "CodeTree : AI 로봇 청소기 (삼성 2025 하반기 오후 1번 문제)"
date: 2025-09-30 20:12:00 +0900
categories: [Algorithm, Problem Solving]  
tags: [impl, bfs, pathfind, simulation]    
toc: true
comment: false
published: true
image:
    path: "https://pbs.twimg.com/media/GgemwJ8WYAA33ew.jpg"
---

[코드 트리 : 2025 하반기 오후 1번 문제](https://www.codetree.ai/ko/frequent-problems/samsung-sw/problems/ai-robot/)
 
2025 삼성 하반기 SW 역량테스트 오후 1번 문제 

구현 및 시뮬레이션 문제

---

## 문제 분석


<img src="https://contents.codetree.ai/problem_factory/images/8e98f3ca-5a97-49fe-8163-ffb1f767ab35.webp" width="500">

- N * N 맵에 로봇 청소기가 돌아다님
- 각각의 격자에는 먼지가 있거나, 먼지가 없거나, 물건이 위치할 수 있음
- 먼지가 있는 공간은 1에서 100 사이의 먼지의 양 P를 가짐
- 각각의 로봇 청소기는 초기 위치를 가지며, 해당 위치에는 먼지가 없음

로봇 청소기 테스트를 진행하는는데 다음 과정을 L번 반복한다


<img src="https://contents.codetree.ai/problem_factory/images/965f8c64-740a-4763-b0ed-5f476946a412.webp" width="500">

- 1) 청소기 이동
	- 각각의 로봇 청소기는 순서대로 이동 거리가 가장 가까운 오염된 격자로 이동
	- 물건이 위치한 격자나, 청소기가 잇는 격자로는 지나갈 수 없음
	- 가장 가까운 격자가 여러개일 경우, 행 번호가 작은 격자로 이동하고, 행 번호가 같은 경우, 열 번호가 같은 격자로 이동

<img src="https://contents.codetree.ai/problem_factory/images/eb8b6781-004c-4fec-91bb-07f71bee12cf.webp" width="500">

<img src="https://contents.codetree.ai/problem_factory/images/320070e2-0d76-46db-9804-62b6b62b3cd8.webp" width="500">

- 2) 청소
	- 청소기는 바라보고 있는 방향을 기준으로, 본인이 위치한 격자, 지신의 왼쪽, 앞쪽, 오른쪽을 청소할 수 있음
	- 청소할 수 있는 4가지 격자에서 청소할 수 있는 먼지량이 가장 큰 방향에서 청소를 시작함
	- 격자마다 청소할 수 있는 최대 먼지량은 20
	- 합이 같은 방향이 여러개인 경우, 동, 남, 서, 북 우선순위로 방향을 선택한다
	- 청소는 청소기마다 순서대로 진행됨
	- 청소기는 자기가 서있는 땅에 먼지가 없어도 청소를 수행함


<img src="https://contents.codetree.ai/problem_factory/images/320070e2-0d76-46db-9804-62b6b62b3cd8.webp" width="500">

- 3) 먼지 축적
	- 먼지가 있는 모든 격자에 동시에 5씩 추가됨


<img src="https://contents.codetree.ai/problem_factory/images/22a4738c-8958-4102-908d-f10af657e8fc.webp" width="500">

- 4) 먼지 확산
	- 깨끗한 격자 주변에 4방향 격자의 먼지량의 합을 10으로 나눈 값 만큼 먼지가 확산됨
	- 나눗셈 과정에서 생기는 소숫점 아래수는 버림
	- 모든 깨끗한 격자에 대해 동시에 확산이 이루어짐

- 5) 출력
	- 전체 공간의 총 먼지량을 출력함
	- 먼지가 있는곳이 없으면 0을 출력함

### 입력

- 첫 줄에 격자 크기 N, 로봇 청소기의 개수 K, 테스트 회수 L이 공백으로 구분되어 주어짐

- 그 다음줄 부터 N줄에 걸쳐 각 행에 해당하는 격자의 먼지 양(p)가 공백을 사이에 두고 주어짐
	- -1 인 경우, 물건이 위치한 격자를 의미함

- N+1줄부터는 K줄에 걸쳐 로봇 청소기의 위치 정보가 주어짐
	- i번째 줄에는 i번쨰 추가되는 로봇 청소기의 좌표 (r_i, c_i)가 공백으로 구분되어 주어짐
- 제한 조건
	- (2 <= N <= 30)
	- (1 <= K <= 50)
	- (1 <= L <= 50)
	- (1 <= r_i, c_i <= N)
	- (-1 <= p <= 100)
- 제한
	- 시간 : 500ms
	- 메모리 : 64MB

### 출력

- 각각의 테스트가 끝날때마다 공간에 있는 먼지의 합을 줄마다 출력함

---

## 알고리즘

구현량이 많고, 복잡해보이지만, 시퀸스별로 나눠놔서 각 행동을 각각 구현 후 합치면 됨

1. 어디로 이동할 것인지 (BFS + 타이 브레이크)

	- 각 로봇은 BFS로 도달한 칸들 중에서, 가장 가까운 오염칸으로 이동한다

	- 거리가 같으면 y가 작은순, y도 같으면 x도 같은 순으로 **타이 브레이크**를 적용하여 목표를 결정해야함

2. 로봇의 위치도 이동 불가능함

	- 로봇이 한번에 하나씩 움직이므로, 다른 로봇에 경로가 막혀있으면 이동 불가능함

	- BFS 충돌 판정시, 방문처리에 로봇이 있는 곳도 체크해둬야함

3. 청소 방향 선택

	- 4방향을 살펴보면서 어딜 청소할지 결정하고, 그 방향을 청소해야함

	- 각 칸당 최대 20씩 청소할 수 있으므로 합을 계산할 때 주의

	- 또한 동 남 서 북 순 으로 우선순위가 있음

```
1. 전처리
	- N, K, L 입력받기
2. 다음 과정을 L번 반복한다
	2-0. 필요시 초기화
	2-1. 청소기 이동
	2-2. 청소
	2-3. 먼지 축적
	2-4. 먼지 확산
	2-5. 출력
```

먼지 축적이나, 확산은 간단한 구현이므로 그냥 하면 된다

2-1. 청소기 이동은 BFS로 처리하면 된다

가장 핵심이 되는 부분은 "2-2. 청소" 부분인데, 

2-2-1. 어느 부분을 청소할 것인지를 판단하는 로직

2-2-2. 해당 부분을 청소하여 현재 맵에 적용시키기

로 나눌수 있을 것 같다.

그 외 부분은 소스코드에 주석으로 설명을 달아놨음 

### 필요 변수

- int N, K, L 
- int map[31][31] : 맵 정보 기록용
- bool visited[31][31] : BFS 처리시 방문처리용
- struct Robot{int y, int x} : y, x 좌표를 가지는 로봇 구조체
- vector<Robot> cleaners : 청소기들을 순서대로 기록하는 벡터

---

## 소스코드

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <cstring>

using namespace std;

const int MAX_SIZE = 35;

int N, K, L;
int map[MAX_SIZE][MAX_SIZE];
int visited[MAX_SIZE][MAX_SIZE];

int direct[4][2] = { {-1,0}, {0, -1}, {1, 0}, {0, 1} }; 
int cleaning_areas[4][4][2] = {
	{\{0,0}, {0,1}, {-1,0}, {1,0}}, // 0 동쪽 : 자신, 앞(동), 왼쪽(북), 오른쪽(남)
	{\{0,0}, {1,0}, {0,1}, {0,-1}}, // 1 남쪽 : 자신, 앞(남), 왼쪽(동), 오른쪽(서)
	{\{0,0}, {0,-1}, {1,0}, {-1,0}}, // 2 서쪽 : 자신, 앞(서), 왼쪽(남), 오른쪽(북)
	{\{0,0}, {-1,0}, {0,-1}, {0,1}}, // 3 북쪽 : 자신, 앞(북), 왼쪽(서), 오른쪽(동)
};

struct Robot {
	int y;
	int x;
};

struct State {
	int dist, y, x;

	// 우선순위 큐의 정렬 기준은 최소힙이므로 > 연산자 사용
	bool operator>(const State& other) const { // const 없으면 c2678 에러 발생함
		// const : 이 멤버 함수는 객체의 데이터를 절대 변경하지 않음을 보장함
		// const 빼먹으면 컴파일러가 데이터 변경할수도 있다고 판단해서 에러 발생시킴
		if (this->dist != other.dist) {
			return this->dist > other.dist;
		}
		if (this->y != other.y) {
			return this->y > other.y;
		}
		return this->x > other.x;
	}
};

vector<Robot> cleaners;

void Input() {
#ifdef _DEBUG
	freopen_s(new FILE*, "input.txt", "r", stdin);
#endif
	cin >> N >> K >> L;
	for (int y = 1; y <= N; y++) { // 1-idx 사용
		for (int x = 1; x <= N; x++) {
			cin >> map[y][x];
		}
	}
	for (int i = 0; i < K; i++) {
		int yy, xx; cin >> yy >> xx;
		cleaners.push_back({ yy , xx }); 
	}
}

void MoveAllRobots() {
	// i 번째 로봇을 움직이는 함수
	for (int i = 0; i < K; i++) {
		// 만약 현재 있는곳이 더럽다면 움직일 필요 없음
		if (map[cleaners[i].y][cleaners[i].x] > 0) {
			continue; 
		}
		memset(visited, 0, sizeof(visited));

		/*
		일반 BFS로는 불가능
		최단거리는 보장하지만, 동일 거리의 여러 후보가 있는 경우 
		누가 먼저 들어가느냐는 부모노드와 방향 순서에 따라 달라짐
		따라서 단순히 direct 순서로는 원하는 정렬 순서를 보장하지 못함
		-> 계층 BFS 혹은 우선순위 큐를 사용해야함
	
		ex)
		로봇이 (3,3)에 있고, 목표 후보가 (2,4), (4,2)에 있는 경우
		- 두 칸 모두 거리는 2
		- 문제 명세에서는 4좌표가 작은순이므로 (2,4)가 정답임
		- 그러나 BFS 큐에 들어가는 순서는 (3,2) → (2,2) → (2,3) → (2,4) … 같은 경로에 따라 달라짐
		- 결국 큐가 먼저 들어간 후보가 "발견된 순간" break 되버리면, 정답이 4,2가 선택되는 경우가 생길 수 있음
		*/

		priority_queue<State, vector<State>, greater<State>> pq;
		pq.push({ 0, cleaners[i].y, cleaners[i].x });
		for (int j = 0; j < K; j++) {
			if (i == j) continue; 
			// 현재 로봇의 위치는 방문처리 하지 않음 -> 
			// 우선순위 큐에서는 Mark on Push 방식 대신, Mark on Pop 방식 사용하기 때문
			// 여기서 방문체크해버리면 큐가 돌아가지 않는 문제 발생함
			visited[cleaners[j].y][cleaners[j].x] = true;
		}

		while (!pq.empty()) {
			State now = pq.top();
			pq.pop();

			// Mark on Pop 방식 사용. 여기서 방문체크를 한다
			if (visited[now.y][now.x] == true) {
				continue;
			}
			visited[now.y][now.x] = true;

			// 목표 찾은경우
			// 시작점이 아닌 먼지칸이라면, 로봇을 그 좌표로 이동시킴
			if (now.dist > 0 && map[now.y][now.x] > 0) {
				visited[now.y][now.x] = false; // 기존 자리는 비워줌
				cleaners[i].y = now.y;
				cleaners[i].x = now.x;
				break; 
			}

			for (int t = 0; t < 4; t++) {
				int dy = direct[t][0] + now.y;
				int dx = direct[t][1] + now.x;
				if (dy < 1 || dx < 1 || dy > N || dx > N) continue; // 경계 처리
				if (map[dy][dx] == -1) continue; // 물건 처리
				if (visited[dy][dx]) continue; // 성능 향상을 위한 방문처리. 

				pq.push({ now.dist + 1, dy, dx });
			}
		}
	}
}


// 해당 좌표의 먼지를 가져오는 함수
int get_dust(int y, int x) {
	// 맵 경계 벗어나는 경우
	if (y < 1 || x < 1 || y > N || x > N) return 0;
	if (map[y][x] == -1) return 0;
	return map[y][x];
}

// 현재 로봇의 좌표에서 어느 방향으로 청소할지 정하는 함수
// 0, 1, 2, 3 : 동 남 서 북 순
// 청소할 필요가 없다면 -1을 반환
int CheckDustAmount(int idx) {
	int now_y = cleaners[idx].y;
	int now_x = cleaners[idx].x;

	/*if (map[now_y][now_x] == 0) {
		return -1;
	}*/

	int max_dust_sum = -1;
	int best_dir_idx = -1;

	// 4방향을 순회하면서 먼지의 합을 계산하고, 최적의 방향을 찾는다


	for (int t = 0; t < 4; t++) {
		int cur_dust_sum = 0;
		for(int p=0; p<4; p++){
			int dy = cleaning_areas[t][p][0];
			int dx = cleaning_areas[t][p][1];

			cur_dust_sum += min(20, get_dust(now_y + dy, now_x + dx));
		}

		if (max_dust_sum < cur_dust_sum) {
			max_dust_sum = cur_dust_sum;
			best_dir_idx = t;
		}
	}

	return best_dir_idx;
}

void CleanAllRobots() {
	// i 번째 로봇이 청소하는 함수
	for (int i = 0; i < K; i++) {
		int target_dir = CheckDustAmount(i);

		// 청소할 필요가 없는 경우, 다음 로봇으로 넘어감
		if (target_dir == -1) {
			continue;
		}

		for (int p = 0; p < 4; p++) {
			int dy = cleaning_areas[target_dir][p][0];
			int dx = cleaning_areas[target_dir][p][1];

			int ny = cleaners[i].y + dy;
			int nx = cleaners[i].x + dx;

			// 경계 및 장애물 체크
			if (ny < 1 || nx < 1 || ny > N || nx > N) continue; 
			if (map[ny][nx] == -1) continue;

			map[ny][nx] = max(0, map[ny][nx] - 20);
		}
	}
	
}

void AddDustAndSpreadDust() {
	// 3. 먼지 축적
	// 먼지가 있는 모든 격자에 동시에 5씩 추가됨
	for (int y = 1; y <= N; y++) {
		for (int x = 1; x <= N; x++) {
			if (map[y][x] > 0) {
				map[y][x] += 5;
			}
		}
	}
	// 4. 먼지 확산
	/*- 깨끗한 격자 주변에 4방향 격자의 먼지량의 합을 10으로 나눈 값 만큼 먼지가 확산됨
		- 나눗셈 과정에서 생기는 소숫점 아래수는 버림
		- 모든 깨끗한 격자에 대해 동시에 확산이 이루어짐
		- 먼지 확산은 동시에 일어나므로, 동시성 문제 방지를 위해 별도의 배열을 두고 이를 한번에 더하는 방식을 사용함
	*/
	int adder[MAX_SIZE][MAX_SIZE] = { 0 };

	for (int y = 1; y <= N; y++) {
		for (int x = 1; x <= N; x++) {
			
			if (map[y][x] != 0) continue; // 빈 셀이 아닌경우 : 물건이 있거나, 이미 먼지가 있는 경우
			
			int total = 0;

			for (int t = 0; t < 4; t++) {
				int dy = direct[t][0] + y;
				int dx = direct[t][1] + x;
				if (dy < 1 || dx < 1 || dy > N || dx > N) continue; // 1-idx라 경계처리 안해도 되지만 혹시 모르니 경계처리
				if (map[dy][dx] == -1) continue;  // 다음 셀이 물건인 경우

				total += map[dy][dx]; 
			}
			adder[y][x] = total / 10; 

		}
	}

	for (int y = 1; y <= N; y++) {
		for (int x = 1; x <= N; x++) {
			map[y][x] += adder[y][x];
		}
	}
}

void PrintTotalDust() {
	int res = 0;
	for (int y = 1; y <= N; y++) {
		for (int x = 1; x <= N; x++) {
			if (map[y][x] > 0) {
				res += map[y][x];
			}
		}
	}
	cout << res << '\n';
}

int main() {
	Input();

	for (int i = 1; i <= L; i++) {
		// 2 - 1. 청소기 이동
		MoveAllRobots();

		// 2 - 2. 청소
		CleanAllRobots();

		// 2 - 3. 먼지 축적
		// 2 - 4. 먼지 확산
		AddDustAndSpreadDust();
		
		// 2 - 5. 출력
		PrintTotalDust();
		int de = -1;
	}

	return 0;
}
```


## 배운점

1. 구조체로 `operator<`를 오버로딩할 때 꼭 `const`를 넣어줘야 에러가 발생하지 않는다. 

	아니면 별도로 비교 함수 객체를 만들어서 비교해야함

2. 우선순위 큐를 이용한 BFS 사용

	해당 문제에서는 우선순위가 거리순 > y좌표 낮은순 > x좌표 낮은순 이런식으로 되어있음

	이를 일반 BFS로는 해결이 불가능하다

	최단거리는 보장하지만, 동일 거리의 여러 후보가 있는 경우 

	누가 먼저 들어가느냐는 부모노드와 방향 순서에 따라 달라짐

	따라서 단순히 direct 순서로는 원하는 정렬 순서를 보장하지 못함

	그러므로, 계층 BFS 혹은 우선순위 큐를 사용해야한다

	이번에는 우선순위 큐를 통해 우선순위를 비교하도록 만든 뒤 먼저 pop되도록 했다

3. 우선순위 큐를 활용한 BFS(Prioirty BFS)에서는 **Mark on Pop** 방식을 사용한다

	일반 BFS에서 큐에 집어넣을때 방문처리를 했다면

	우선순위 큐에서는 팝되어 나올때 방문처리를 해야한다

	미리 방문처리를 해버리면, 큐가 돌아가지 않는 문제가 발생할 수도 있기 때문이라고 함

	평소에 어디에 두나 별 상관없는줄 알았으나 이런 미세한 차이가 있을수도 있다고 함