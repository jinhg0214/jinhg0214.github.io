---
title: "원을 그리는 방법"
# excerpt : 요약
date: 2023-11-07 14:37:25 +0900
# last_modified_at: 
categories: [Algorithm, Problem Solving]  # 최대 2개 가능
tags: [circle, computergraphics, midpoint, bresenham]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

Q. 한 점 (y, x)가 주어질 때, 이 점이 어느 원 안에 들어있는지 어떻게 판별할 것인가

1. 수학적 공식을 이용해 원에 속하는지 판별한다

2. 직접 그리고 그 좌표를 확인한다

---
# 1. 수학적 공식 이용

`d = x^2 + y^2 - r^2` 을 이용해 

d가 0보다 크면 원 외부, d가 0보다 작으면 원 내부로 판단한다

### 원리 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ed03d620-055c-4b88-b510-b3c6ae95affe)

`(tx - x)^2 + (ty - y)^2` 가 `r^2`보다 크다면, 원의 바깥이고, 작다면 원의 내부이다

그러나 이 방법은 제곱 연산을 세번씩 수행해야함

확인해야하는 점이 12개라면 36번 수행.(r^2을 변수로 할당해서 25번으로 줄일 수 있긴함 )

# 2. 직접 그리고 그 좌표를 확인한다
---
**브레즌햄 미드 포인트 알고리즘(bresenham midpoint circle algorithm)**을 이용해 원을 그린다

픽셀 배열을 정의하고, 원을 이 셀에 그린 뒤,

그 셀의 값을 확인하는 방법

![다운로드](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/4b638cb0-1623-4a8e-952e-44279df492fa)

픽셀아트나, 마인크래프트같은 게임에서 원 그릴때 쓰는 방법

정확한 원을 그리는것은 불가능하므로, 원과 비슷하게 그린다

이를 이용해 원을 판별

브레즌햄 직선 알고리즘을 개발하신 그 브레즌햄이 만들었다

### 원리

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/9207ef71-d78b-4b28-a4bc-0184c358cc80)

1. 원을 8조각으로 쪼갠다
2. (x, y)가 해당하는 첫번째 호를 그리면, 다른 호는 좌표면 바꾸면 쉽게 그릴 수 있음

그 첫번째 호를 어떻게 그릴것인가

### 첫번째 호를 그리는 방법

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/7ec02bf6-0996-4c1b-a0ea-43e9122c65e9)

- 결정된 한 점 `(x, y)`가 있을 때, 한칸 옆인 `x + 1`의 값이 `y`, `y -1` 인지 판단함
    - 나랑 같은 높이인지, 나보다 낮은 높이인지를 판별

	- 이 두가지 후보 중 하나를 정하는 과정에서 **중간점(Midpoint)** 을 사용함

	- 중간점`(x+1, y-1/2)가 `이 원 안에 있다면 `y -1`을, 원 바깥에 있다면 `y`를 선택함

	- 이를 식으로 나타내면 `p_k = (x + 1) ^ 2 + (y - 0.5)^2 - r^2`

- P_(k+1)에 대해 점화식으로 정리하면 다음과 같이 정리할 수 있다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/1d1e1ad0-a5d9-4ed7-8fe9-957de3b9e874)

p_k 가 0보다 작으면 y_k를, 0보다 같거나 크면 y_k -1 을 이용하므로


```
p_k >= 0 : 
	p + 2(x - y) + 3
else :
	p + 2x + 1
```

로 정리할 수 있다

시작 지점은 원의 중심 (X, Y)로 부터 윗방향인 (0, r)부터 시작하면

P_0 는 `5/4 - r` 이 나오나, 정수형 연산을 위해 `1-r`로 사용

이를 정리하면 다음과 같다
```
1. 초기 지점 `(x, y) = (0, r)`에서 시작. Decision parameter는 `p = 1-r`
2. p가 0보다 작다면 : 
	- (x_k, y_k) = (x+1, y) : 
	- p_k = p + 2x +1
	p가 0보다 같거나 크다면:
	- (x_k, y_k) = (x+1, y-1)
	- p_k = p + 2(x - y) + 3
3. 이를 x < y 인 동안 반복한다 (첫번째 호를 다 그릴때 까지)
4. 첫번째 호의 좌표가 모두 얻어졌으면, 이를 이용해 나머지 7개 호를 그린다
```

### 예시
중심이 (5, 7)이고, 지름이 12인(r = 6) 원 그리기

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/7cc69f36-1e5a-4eab-8284-f873becfd6ff)

P : Decision parameter, X, Y 좌표, (X_plot, Y_plot) : 디스플레이에 넣을 픽셀의 위치 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/c6d400c5-6acb-4814-803e-0666c65d9894)

첫번째 열을 채우면 이렇게 됨
- P = 1-r 이므로, -5
- P가 0보다 작으므로, X = X + 1, Y = Y
- (X_plot, Y_plot) = (5 + 1, 7 +6) 이므로 (6,13)에 점을 찍는다
- Decision Parameter를 갱신한다. p가 0보다 작았으므로, `p + 2x + 3 = -2`

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/45310673-1a99-4fdc-91d2-c8d844b11385)

이런식으로 채우다가 X >= Y 인 5번째 줄에서 종료한다

(x, y)는 중심에서의 거리, (x_plot, y_plot)은 실제 점을 찍을 좌표가 저장된다

이를 이용해 원을 그리고, 내부를 채우면

이 원에 속하는지 쉽게 판별할 수 있다 (2차원 배열이므로 O(n))

# 소스코드

```cpp
#include <iostream>
#include <vector>

using namespace std;

#define MAX 28

struct Point {
	int y;
	int x;
};

// 맵 출력함수
void showMap(int map[MAX][MAX]) {
	for (int y = 0; y < MAX; y++) {
		for (int x = 0; x < MAX; x++) {
			if (map[y][x] == 1) {
				cout << "■";
			}
			else if (map[y][x] == 2) {
				cout << "◆";
			}
			else if (map[y][x] == 3) {
				cout << "▣";
			}
			else if (map[y][x] == 4) {
				cout << "◈";
			}
			else if (map[y][x] == 5) {
				cout << "▲";
			}
			else if (map[y][x] == 6) {
				cout << "▼";
			}
			else if (map[y][x] == 7) {
				cout << "♣";
			}
			else if (map[y][x] == 8) {
				cout << "♥";
			}
			else if (map[y][x] == 9) {
				cout << "☆";
			}
			else {
				cout << "　"; // space가 아니라 "　" ㄱ 한자 1임. 
			}
		}
		cout << endl;
	}
}

// 배열, 반지름, 중심좌표 (y, x)를 받아 원을 그리는 함수
void drawCircle(int map[MAX][MAX], int r, Point point) {
	int x, y;
	int p;
	vector<Point> v;

	// 초기 지점 (0, r), p 세팅
	x = 0;
	y = r;
	p = 1 - r;

	// 벡터에 첫번째 호의 좌표들을 모두 넣고 이를 8번 돌리면서 그린다
	v.push_back({ y, x}); // (0, r)

	// 여기서부터 미드포인트 시작
	while (x < y) {
		if (p < 0) {
			x = x + 1;
			y = y;
			p = p + 2 * x + 3;
		}
		else {
			x = x + 1;
			y = y - 1;
			p = 2*(x - y) + 5;
		}
        // y, x좌표를 벡터에 쭉 저장한다
		v.push_back({ y, x});
	}

	// 완료됬다면 , 점이 첫번째 호가 그려진 것이므로, 이를 8번 돌리면 됨
	// (x, y), (x, -y), (-x, y), (-x, -y), (y, x), (y, -x), (-y, x), (-y, -x)
	for (auto e : v) {
		map[e.y + point.y][e.x + point.x] = 1;
		map[e.y + point.y][-e.x + point.x] = 2;
		map[-e.y + point.y][e.x + point.x] = 3;
		map[-e.y + point.y][-e.x + point.x] = 4;
		map[e.x + point.y][e.y + point.x] = 5;
		map[e.x + point.y][-e.y + point.x] = 6;
		map[-e.x + point.y][e.y + point.x] = 7;
		map[-e.x + point.y][-e.y + point.x] = 8;
	}
}

// 배열, 반지름, 중심좌표 (y, x)를 받아 원을 채우는 함수
void fillCircle(int map[MAX][MAX], int r, Point point) {
	int x, y;
	int p;
	vector<Point> v;

	// 초기 지점
	x = 0;
	y = r;
	p = 1 - r;

	v.push_back({ y, x });
	while(x < y) {
		if (p < 0) {
			x = x + 1;
			y = y;
			p = p + 2 * x + 3;
		}
		else {
			x = x + 1;
			y = y - 1;
			p = 2 * (x - y) + 5;

            /*
            원을 그리는 함수에서 y, x를 넣을때 
            (y,0) ~ (y,x)까지 다 넣어주는 것 외에는 모두 같다
            */
			for (int i = 0; i < x; i++) {
				v.push_back({ y, i });
			}

		}
		v.push_back({ y, x });
	}
	for (auto e : v) {
		map[e.y + point.y][e.x + point.x] = 1;
		map[e.y + point.y][-e.x + point.x] = 2;
		map[-e.y + point.y][e.x + point.x] = 3;
		map[-e.y + point.y][-e.x + point.x] = 4;
		map[e.x + point.y][e.y + point.x] = 5;
		map[e.x + point.y][-e.y + point.x] = 6;
		map[-e.x + point.y][e.y + point.x] = 7;
		map[-e.x + point.y][-e.y + point.x] = 8;
	}
	for (int i = point.y - y + 1; i < point.y + y; i++) {
		for (int j = point.x - x + 1; j < point.x + x; j++) {
			map[i][j] = 9;
		}
	}
}


int main() {
	int map[MAX][MAX] = { 0 };

	// (13, 11)에 반지름이 11인 원 그리기
	// drawCircle(map, 11, { 13, 11 });
	fillCircle(map, 11, { 13, 11 });
	showMap(map);

	return 0;
}

```

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ca2339e2-142d-46dd-8c1f-fcc2655d4e9e)

drawCircle 로 원만 그린 모습

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/0f9006f7-bebc-4be9-b473-b44c029db8a9)

fillCicle로 내부까지 채운 모습

---

참고 :

https://en.wikipedia.org/wiki/Midpoint_circle_algorithm#:~:text=In%20computer%20graphics%2C%20the%20midpoint,and%20Van%20Aken.

https://tibyte.kr/253#comment14540588

https://husk321.tistory.com/250

https://www.youtube.com/watch?v=jMTSwnsLY1A

https://m.blog.naver.com/kch8246/220827144259