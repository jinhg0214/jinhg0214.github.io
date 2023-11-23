---
title: "프로그래머스 레벨2 리코쳇 로봇"
date: 2023-11-22 21:24:00 +0900
# last_modified_at: 
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [bfs, bruteforce, impl]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

프로그래머스 레벨2 연습문제

장애물이나 맨 끝에 부딪힐 때까지 미끄러져 이동을 통해 최단거리 구하기

기존의 BFS에서 한번 더 업그레이드 된 문제

[https://school.programmers.co.kr/learn/courses/30/lessons/169199](https://school.programmers.co.kr/learn/courses/30/lessons/169199)

# 1. 문제
---
### 간단 설명

![qjY49Ra5m3C_DZNEaFbxsWkLuPuQW5Jw7FxbDprponQ](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/54e83839-36c3-4525-9785-a59c113514d1)

포켓몬스터 얼음 퍼즐처럼 움직인다

장애물('D')이나, 맨 끝에 부딪힐 때 까지 미끄러진다

BFS를 이용해서 최단거리를 탐색할 때,

언제 방문처리를 할 것인지,

dy, dx를 어떻게 갱신할 것인가가 포인트


# 2. 문제 분석
---
### 필요변수
`vector<string> board`로 입력이 주어짐

기본적인 변수들은 BFS와 같음

맵 정보를 저장할 `char map[y][x]`, 방문표시 처리를 할 `int visited[y][x]`

좌표 구조체 `struct node` 대신 `pair<int, int>`를 이용해 좌표 처리

4방향 표현을 위한 `direct[4][2]`

시작 로봇의 위치와 도착점 등


### 주의점
1. 입력이 알파벳 문자열로 주어짐
    - 이를 잘라서 int 배열에 잘 변환해서 넣을것
2. 기존의 BFS에서는 한칸씩 움직였지만, 이번에는 쭉 미끄러지는걸 한번의 이동으로 침
    - 이를 어떻게 구현할 것인지가 포인트
    - 또한 방문 처리를 언제 할 것인지가 포인트

### 알고리즘
```
1. 입력받기
2. 로봇의 시작지점을 방문처리후, 이 지점부터 BFS 시작
3. BFS 시작. 큐에서 맨 앞을 꺼낸다
    3-1. dy, dx 를 큐에서 꺼낸 now의 좌표로 설정
    3-2. 이 좌표(dy, dx)에 y, x 갱신값을(direct[t]) 경계, 또는 벽을 만날때까지 계속 더해본다
    3-3. 벽 또는 경계를 만났다면 멈춘다
    3-4. 이 dy, dx가 목표지점인지 체크
        - 목표 지점이라면 이동거리를 리턴
    3-5. 이미 방문한곳이라면 패스
    3-6. 방문처리 후, dy, dx, 레벨 + 1 을 큐에 넣는다 
```


# 3. 소스코드
---
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <queue>

using namespace std;

#define MAX 101

char map[MAX][MAX]; // 맵 저장용
int visited[MAX][MAX]; // 방문처리 + 몇번 이동했는지 체크용
int N, M; // 맵 사이즈 저장용 
int start_y, start_x, dest_y, dest_x; // 시작지점과 도착지점 표시 
int direct[4][2] = {0,1,1,0,0,-1,-1,0};

int BFS() {
	queue<pair<int, int>> qu;
	qu.push({ start_y, start_x });
	visited[start_y][start_x] = 1;

	while (!qu.empty()) {
		pair<int, int> now = qu.front();
		qu.pop();
	
		if (now.first == dest_y && now.second == dest_x) {
			return visited[now.first][now.second] ;
		}

		for (int t = 0; t < 4; t++) {
			int dy = now.first;
			int dx = now.second;

			while (true) {
				dy += direct[t][0];
				dx += direct[t][1];

				if (dy < 0 || dx < 0 || dy >= N || dx >= M) break;
				if (map[dy][dx] == 'D') break;
			}

			dy -= direct[t][0];
			dx -= direct[t][1];

			if (visited[dy][dx]) continue;

			visited[dy][dx] = visited[now.first][now.second] + 1;
			qu.push({ dy, dx });
		}
	}
	return 0;
}

int main(){
    // 비쥬얼 스튜디오에서 코딩해서 이렇게 수정했음 
	freopen_s(new FILE*, "input.txt", "r", stdin);
	vector<string> board = {
		"...D..R",
		".D.G...",
		"....D.D",
		"D....D.",
		"..D...."
	};

    // 입력받기
	N = board.size();
	M = board[0].size();
	for (int y = 0; y < N; y++) {
		for (int x = 0; x < M; x++) {
			map[y][x] = board[y][x];
			if (map[y][x] == 'R') {
				start_y = y;
				start_x = x;
			}
			else if (map[y][x] == 'G') {
				dest_y = y;
				dest_x = x;
			}
		}
	}
    // BFS 
	cout << BFS() - 1; 
    // -1 해주는 이유?
    // 따로 이동횟수를 카운팅하는 변수를 사용하지 않고, 방문체크 배열에 같이 기록함
    // 첫 시작부터 한칸으로 체크되므로, 도착시 -1 해줘야 올바른 값이 출력됨
	return 0;
}

```

프로그래머스에서 IDE 없이 직접 푸는게 아직 익숙하지 않아

어려운데 더 어려워졌다

IDE를 사용하지 않고 디버깅하는 방법에 익숙해져야할듯 하다