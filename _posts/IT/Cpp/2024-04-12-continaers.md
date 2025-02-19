---
title: "C++ Containers"
date: 2024-04-12 05:27:00 +0900
categories: [C++, STL]  # 최대 2개 가능
tags: [container, array, list, vector, deck, set, map]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://i.stack.imgur.com/3gBcx.png"
    alt: 
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/6OoSgY6NVVk?si=HbovH1YCYcaKOiAZ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- STL Container 관련해 좋은 영상이 있길래 정리해봄
- 어느정도는 알고있는 내용이 많았지만, 몰랐던 부분들도 많았다. 특히 Deck 부분
- 고수들이 코드 어떻게 짜는지를 볼 수 있었음

## 1. Array
---
- 가장 기본적인 배열
- 배열은 크기 resize가 불가능함

![1  Array](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/8980d478-4c74-42f6-a8ff-900858df938d)

- Contiguous 속성을 가짐
	- 접근 속도가 매우 빠르다
	- `a[7] = a + 7*sizeof(int)` 와 같이 접근 가능
- 배열의 크기를 벗어나면 Array index out of bound behavior 에러 발생

```cpp
int a[10]; // Stack에 연속으로 할당됨, stack은 scope를 벗어나면 자동으로 제거됨

int *b = new int[10]; // Heap으로 runtime에 할당
delete[] b; // 메모리 누수를 의해 Heap으로 할당한 메모리는 꼭 지울 것
```

## 2. Standard Array
---

```cpp
#include <array>

int a[10];
a[17] = 6; // 에러가 발생할수도, 아닐수도 있음. 컴파일러 디버거에 따라 다름

std::array<int, 10> b; // std 선언 방식
b[17] = 6; // array out of range 에러 발생 후 종료시킴. 안전함!

b.size()
b.data();
```
![1 Standard_array error](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/b25ccbfe-692d-4de4-9e36-171ee8c1157a)
*visual studio가 경고를 띄우긴 하지만 실행은 됨*

![1 Standard_array error2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/1ccbc2ad-94c6-4f35-80f8-23a12c394e40)
*에러를 출력하고 뻗음*

- 컨테이너는 데이터들을 추상화를 통해 연속적으로 할당함
- 이를 iterator로 추적할 수 있음


## 3. Iterator
---

- 컨테이너의 요소 중 하나를 식별하는 방법
- iterator를 사용하는 방법

```cpp
std::array<int, 10> b;
for(int i=0; i<b.size(); i++){
    b[i] = something();
}

// iterator를 사용하는 방법
for(std::array<int, 10>::iterator i = b.begin(); i != b.end(); ++i){
    *i = something();
}

for(auto i = b.begin(); i != b.end(); ++i){
    *i = something();
}

for(auto& i : b){
    i = something();
}

```
- algorithm라이브러리의 fill 사용   

```cpp
#include <algorithm>

std::fill(b.begin(), b.end(), something());
```

- 혹은 직접 할당 `b = {1,2,3,4,5,6,7,8,9,0};`

## 4. Standard Vector
---

- 동적으로 변하는 배열
- 배열과 유사함

![4  vector](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/34074b8b-e4a9-408e-888b-2d90915a1657)


- 벡터는 크기가 더 필요하면 contigious를 유지하기 위해서 메모리를 재할당 받음
- 이 재할당 받은 메모리에 모든 내용을 이주시킴
- 그러나 `int *p`가 이전 메모리 주소를 가리키고 있다면?
    - junk를 가리키게 됨
    - 그러므로, 벡터에 포인터를 쓸 때 주의해야함! 벡터의 크기가 변하지 않는것이 보장된다면 사용 가능

```cpp
#include <iostream>
#include <random>
#include <chrono>
#include <array>

using namespace std;

/*
컨테이너에 메모리를 어떻게 할당하는지 메모리 주소와 시간을 체크하는 프로그램
벡터, 리스트, 덱에 사용 가능
set, map도 조금 수정하면 사용 가능
아래 결과 화면들은 이 프로그램을 사용한 것
*/

int main() {	
	// 주사위를 굴리는 람다 함수
	auto roll = []() { return rand() % 6 + 1; };

	// 컨테이너 생성
	std::vector<int> container;

	// 아이템 추가
	container.push_back(roll());
	const int* pAddressOfOriginalItemZero = &(*container.begin()); // 벡터의 시작지점의 포인터를 저장

	// 시간 측정용으로 chrono 사용
	std::chrono::duration<double> durInsertTime(0);

	// 엔터 입력받는동안 계속 아이템 추가
	do {
		// 첫번째 아이템의 포인터 저장
		const int* pAddressOfItemZero = &(*container.begin());

		std::cout << "Contains " << container.size() << " elements, took " <<
			std::chrono::duration_cast<std::chrono::microseconds>(durInsertTime).count() << "us\n";

		for (const auto& i : container) {
			const int* pAddressOfItemX = &i;
			int pItemOffset = pAddressOfItemX - pAddressOfItemZero;
			int pItemOffsetOriginal = pAddressOfItemX - pAddressOfOriginalItemZero;
			std::cout << "Offset From Original: " << pItemOffsetOriginal << "    Offset From Zero : " << pItemOffset << "    :    Content : " << i << "\n";
		}

		auto tp1 = std::chrono::high_resolution_clock::now();
		container.push_back(roll());
		auto tp2 = std::chrono::high_resolution_clock::now();
		durInsertTime = (tp2 - tp1);

	} while (getc(stdin)); 
	return 0;
}
```

![실행 결과](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/a9838652-f9f1-44a4-8848-66ae080c4f0d)

- 첫 시작시, 벡터의 시작 지점을 0으로, 원소 하나를 추가함
- 두번째 Enter 입력 시, 벡터의 크기를 초과하여 메모리가 재할당 된 것을 볼 수 있음 6932으로 이동함
- offest from zero를 보면 연속적으로 할당되고 있는것을 확인 가능
- 세번째도 마찬가지
- 이런식으로 원소가 추가될 떄 마다 공간이 필요하면 메모리 재 할당 후, 이주시킨다
- 이를 방지하기 위해선 처음부터 크기를 정하는 것도 좋은 방법
- 임의접근, 임의 인덱싱에 매우 적합함

## 5. Lists
---

- 이중 연결 리스트
- Contigious가 보장되지 않음. 메모리에 적당한 위치에 할당 후 포인터로 연결함
- 삽입 및 삭제가 매우 저렴함
- 얼마나 많은 원소가 있는지 확인하기 어려움. 직접 순회해야함 

![list 실행 결과](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/280c2f24-4209-4590-89b7-92e25dbcd807)

- 위의 예시에서 `std::list<int> container;` 로만 변경하여 실행한 결과
- 첫번째 할당된 곳 0 을 기점으로,
- 두번째에는 -624에, 세번째는 -288에 할당되었음

## 6. Decks
---

- Double ended queue, 덱
- 리스트의 삽입 속도를 가지면서, 벡터의 참조 속도를 가짐

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/cab3ccb6-3965-4619-b536-0c2f13e79713)

- 각 블록이 연결된 배열임
- 공간이 부족할 때, 복사할 필요 없이 링크를 추가하면됨
- 그러나 중간에 추가할 때, 약간의 오버헤드가 발생할 수 있음
- 인덱스표를 가지므로, 메모리 오버헤드 발생함
- 벡터만큼 빠르진 않지만 그래도 임의 접근 가능

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/57913973-a08f-451e-8a99-933d09144970)

- 1,2,3,4 번째 원소를 넣을땐 연속되어 할당됨
- 그러나 그 뒤에 원소 4개를 넣을 땐, 새로운 블록을 생성하고 할당하는 모습

## 7. Sets, Unordered Sets
---

### Sets
- 위에 언급한 컨테이너들과는 약간 성질이 다름
- 고유 객체들의 집합
- 몇가지 규칙들이 내포되어있음
    - 중복을 허용하지 않아, 특정 유형의 항목 중 한가지만 가질 수 있음

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/adc087db-ca8b-4dfd-909d-6972fe201116)
- 중복 원소는 추가하지 않지만, 원소 검사를 하는데 어느정도 시간이 소요된다
- 새로운 원소를 추가할 때 마다, 새로 정렬해야하므로 시간이 오래 걸리는 모습

### Unordered Set

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/5fa87f80-2f9f-4d53-8005-6dd7b5c30b80)

- 정렬을 하지않아 속도가 set보다 조금 빠른편
- 많은 데이터를 수집하고, 중복된 데이터가 있을 떄 유용함

## 8.Maps
---

```cpp
std::unordered_map<std::string, int> container; 

container["one"] = 1;
container["two"] = 2;
container["three"] = 3;
container["four"] = 4;
container["five"] = 5;
container["six"] = 6;

for (auto& i : container) {
    std::cout << i.first << " = " << i.second << "\n";
}
```
- key, value 쌍을 가지는 자료구조
- unordered map은 순서없이 저장됨
- map은 key 순서대로 저장됨. 알파벳순이라던지, 숫자 오름차순이라던지 등

