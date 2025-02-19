---
title: "운영체제 9장 - 가상 메모리"
date: 2024-03-28 15:22:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [memory, virtual, paging, page, replacement, thrasing]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://afteracademy.com/images/what-are-the-page-replacement-algorithms-banner-76e2bd3ecfcc77fa.png"
    alt: 
---

# 목차
---

- 9.1 배경 지식
- 9.2 요구 페이징(Demand Paging)
- 9.3 쓰기 시 복사(Copy-on-Write)
- 9.4 페이지 교체(Page Replacement)
- 9.5 Frame 할당
- 9.6 쓰레싱(Trashing)
- 9.7 Memory-Mapped Files


# 9.1 배경 지식
---

- 가상 메모리(Virtual Memory) 
	- 디스크의 일부를 메모리로 사용함
	- 프로그램의 논리 메모리와 물리 메모리를 분리
	- 프로세스가 완전히 메모리에 적재되지 않아도 프로세스 실행 허용
	- 물리적인 메모리보다 큰 프로세스 수행 가능
- 가상 메모리의 구현 방법 두가지
	- 요구 페이징(Demand Paging)과 요구 세그먼테이션(Demand Segmentation)
- 가상 메모리는 프로세스들이 메모리와 파일 공유를 가능하게 함

# 9.2 요구 페이징(Demand Paging)
---

- Page가 필요할 때, Page를 디스크에서 메모리로 가져옴
- 필요한 프로그램만 메모리에 적재한다
- 장점
	- 입출력 감소 -> swap 시간 감소
	- 적은 메모리 사용 -> 공간 절약
	- 빠른 응답 시간
	- 더 많은 사용자 허용
- page table의 valid bit 사용 : 1은 메모리에 있음. 0은 메모리에 없음

### 페이지 부재(Page Fault) 처리

- 페이지가 메모리에 없는 경우 적재하는 과정

![9 2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/57225ab9-3f1b-46eb-ab55-3d3fce3fe785)

```
1. page table의 해당 entry를 조사하여 invalid 원인을 알아냄
	- invalid reference -> abort
	- not in memory, but on the disk -> page in it
2. free(empty) frame을 찾음
3. 디스크에 있는 page를 page frame으로 swap in
4. disk read가 완료되면 page table 갱신
	- 적재된 page의 page table entry에서 valid bit을 1로 설정
5. page fault trap에 의해 중단되었던 instruction을 재시작
```

# 9.3 쓰기 시 복사(Copy-on-Write)
---

1. Copy-on-Write(COW)
	- fork()를 사용한 프로세스 생성시에 자식 프로세스는 부모 프로세스와 같은 페이지를 공유함. 
		- page table 내용 복사. page는 복사하지 않음
		- ![9 3](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/56f69115-0989-4f9d-afbb-72f477d2d994)
	- 공유 페이지를 수정한다면, 수정되는 page만 복사한 후에 수정함
	- 순수 요구 페이징은 요구 받기 전에는 아무것도 적재하지 않음
	- 장점
		- 프로세스 생성 시간 감소
		- 페이지 수 최소화 가능

2. Zero-fill-on-demand
	- free page frame을 할당할 때에, 0으로 채워 이전 내용을 지운 후 할당함

# 9.4 페이지 교체(Page Replacement)
---

- free frame이 없는 경우
```
1. 희생 frame 선택
2. 디스크에 저장
3. 필요한 페이지 가져오기
4. free frame에 적재
5. page table, frame table 갱신(내용이 수정되지 않았다면 저장 불필요)
6. page fault가 발생한, instruction 재시작
```

- 요구 페이징 구현에는 프레임 할당 알고리즘과 페이지 교체 알고리즘 필요
- Frame 할당 알고리즘
	- 여러 프로세스가 존재하는 경우, 각 프로세스에게 얼마나 많은 프레임을 할당할지 결정하는 알고리즘
- 페이지 교체 알고리즘 : 희생 frame을 선택하는 알고리즘
	1. First-In-First-Out(FIFO)

![9 4_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/17f0418e-83b9-4842-95c3-b8070426ed3f)

![9 4_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/6da66297-7e10-4231-8d82-fbafa26b3cfb)

		- Belady's Anomaly 가능성 : page frame 늘려도, page fault가 증가함
	2. Optional Algorithm
	   
![9 4_3](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/08033cf5-3759-4db6-abd9-7feb1e74a102)

		- 가장 오랫동안 사용되지 "않을" page 교체
		- 어떤 페이지가 사용되지 않을지 예측하는건 불가능하므로, 주로 알고리즘 비교 목적으로 사용함
	3. Least Recent Use(LRU), 최소 최근 사용 알고리즘
		- 가장 오래전에 참조된 페이지 교체
		- 단점
			- 하드웨어 지원 counter 필요, 페이지 번호 갱신에 오버헤드
	4. LRU 근사 알고리즘
		- 부가적인 참조 비트를 이용 
		- 페이지 참조시 1, 사용되지 않으면 0. 순서는 알지 못함
		- 참조 비트가 0인 페이지 중 하나 선택 후 교체
	5. LRU 근사 알고리즘  - 2차 기회 알고리즘
		- 참조 비트만 사용
		- 페이지들을 circular queue로 구현
		- 교체될 페이지들을 순서대로 조사
		- 참조 비트가 1이면 0으로 설정 후, 기회를 한번 더 줌
		- 이후 참조 비트가 0이면 해당 페이지 교체
	6. 개선된 LRU 근사 알고리즘
		- 4가지 조합 (Reference bit, modify bit) 사용
	- 그외
		- Counting Algorithm :  각 페이지 엔트리에 참조 횟수 계수를 위한 카운터 사용
		- Page-Buffering Algorithm
		- 수정된 Page-buffering Algorithm
		  
# 9.5 Frame 할당
---
- 프로세스에게 할당하는 프레임 수 
	- 최소 frame 수 : 아키텍쳐에 의해서 결정
	- 최대 frame 수 : 가용 물리 메모리 크기에 읳 ㅐ결정
- 프레임 할당 알고리즘
	1. Fixed Allocation
		- 균등 할당(Equal allocation) : 모든 프로세스에게 똑같이 할당
		- 비례 할당(Proportional allocation) : 프로세스의 크기에 비례하여 할당
	2. Priority  Allocation
		- 프로세스 크기 대신 우선순위 또는 크기 및 우선순위의 조합을 사용한 비례 할당
		- 전역 교체 : 모든 Frame 집합에서 교체할 Frame 선택
		- 지역 교체 : 자신에게 할당된 Frame 집합에서 선택
		  
# 9.6 쓰레싱(Trashing)
---
- Thrasing
	- 프로세스가 page를 충분히 할당받지 못하여 빈번한 page 교체가 발생하여,   
	  프로세스가 반복적으로 swap-in/out 하느라 바쁜 상황
- 지역성 모델(Locality Model)
	- Trashing 방지를 위해 프로세스에게 필요한 만큼의 frame을 제공해야함
	- 지역성 : 집중적으로 함께 사용하는 페이지들의 집합
- 지역교환 알고리즘, 우선순위 알고리즘으로 제한 가능
- 프로세스가 최소로 필요로 하는 프레임만큼만 할당
- page fault frequency scheme : 부족하면 할당, 넘어가면 회수

# 9.7 Memory-Mapped Files
---
- 디스크 블럭 또는 파일들 메모리에 있는 페이지에 맵핑
- 가상 주소의 일부를 논리적 파일과 연관
- 장점
	- 파일 입출력을 메모리 접근을 통해 단순화 가능
	- 여러 프로세스가 페이지를 공유, 같은 파일에 맵핑이 가능함