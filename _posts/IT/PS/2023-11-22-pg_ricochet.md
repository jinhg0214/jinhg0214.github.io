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

맵 정보를 저장할 `int map[y][x]`, 방문표시 처리를 할 `int visited[y][x]`

좌표 구조체 `struct node`

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

int map[101][101];
int visited[101][101];
int y_size;
int x_size;

struct Node {
    int y;
    int x;
    int level = 0;
};

Node robot, target;

int direct[4][2] = { 0,1,1,0,0,-1,-1,0 };

// 입력을 처리하는 함수
// 입력 정보들을 받아 map과 시작지점, 도착지점을 세팅한다
void Input(vector<string> board) {
    y_size = board.size();
    x_size = board[0].size();

    for (int y = 0; y < y_size; y++) {
        for (int x = 0; x < x_size; x++) {
            char ch = board[y][x];
            if (ch == '.') { map[y][x] = 0; }
            // 로봇 시작지점
            else if (ch == 'R') {
                robot.y = y;
                robot.x = x;
                robot.level = 0;
                map[y][x] = 5;
            }
            // 목표 지점
            else if (ch == 'G') {
                target.y = y;
                target.x = x;
                map[y][x] = 9;
            }
            // 벽
            else if (ch == 'D') {
                map[y][x] = 1;
            }
        }
    }
}

// 시작지점부터 목표지점의 최단거리를 찾는 BFS 함수
int BFS() {
    queue<Node> qu;
    qu.push({ robot });
    visited[robot.y][robot.x] = 1; // 시작지점도 넣어주어야 헤메지않음

    while (!qu.empty()) {
        Node now = qu.front();
        qu.pop();

        for (int t = 0; t < 4; t++) {
            // === 이 부분이 제일 중요함======================================================
            int dy = now.y;
            int dx = now.x;
           
            // 경계 또는 벽을 만날때까지 쭉 이동 
            while (true) {
                if (dy < 0 || dx < 0 || dy >= y_size || dx >= x_size) break; // 맵 경계처리
                if (map[dy][dx] == 1) break; // 벽이라면 더이상 진행 불가

                dy += direct[t][0];
                dx += direct[t][1];
            }
            // 경계 또는 벽을 만나서 탈출했기 때문에 다시 한칸 뒤로
            dy -= direct[t][0];
            dx -= direct[t][1];

            // 목표 지점이라면 now의 레벨에 +1 해준다(dy, dx좌표 기준이므로)
            if (dy == target.y && dx == target.x) {
                return now.level + 1;
            }
            // ================================================================================

            if (visited[dy][dx]) continue; // 이미 방문한곳 처리

            visited[dy][dx] = now.level + 1;
            qu.push({ dy, dx, now.level + 1 });
        }
    }
    return -1;
}

int main() {
    int answer = 0;

    vector<string> board = {
        "...D..R",
        ".D.G...",
        "....D.D",
        "D....D.",
        "..D...."
    };

    Input(board);

    cout << BFS();

    return answer;
}
```

프로그래머스에서 IDE 없이 직접 푸는게 아직 익숙하지 않아

어려운데 더 어려워졌다

IDE를 사용하지 않고 디버깅하는 방법에 익숙해져야할듯 하다