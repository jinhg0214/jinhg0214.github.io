---
title: "운영체제 8장 - 메모리 관리 : 스와핑과 메모리 할당"
date: 2024-03-27 15:00:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [memory, swapping, allocation, fragmentation]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://i.stack.imgur.com/dSWgj.gif"
    alt: 
---

# 8.2 스와핑(Swapping)
---

- 실행을 계속할 수 없는 프로세스를 메모리에서 예비 저장 장치(backing store)로 내보내고(swap out),    
	실행할 수 있는 프로세스를 가져옴(swap in)
	- 장점 : 모든 프로세스의 물리적 크기의 합보다 물리적 메모리 크기가 작아도 사용 모두 실행 가능함
- Dispatcher : 다음 프로세스가 메모리, 예비 저장장치 어디에 있는지 확인
- 스와핑 동작

```
1. CPU 스케줄러가 ready queue에서 다음 프로세스를 결정하고 Dispatcher를 호출
2. Dispatcher는 다음 프로세스가 어디에 있는지 확인
	2-1. 메모리에 없고 여유 메모리 영역이 없다면,    
		메모리에 있는 한 프로세스를 내보내고, 원하는 프로세스를 메모리로 불러오기
	2-2. 메모리에 없지만, 여유 메모리 영역이 있다면,   
		원하는 프로세스를 메모리로 불러오기
3. Dispacher는 context switching을 수행
```

- 기본 스와핑, 변형된 스와핑 등 다양한 스와핑 방법 존재

# 8.3 연속(Contiguous) 메모리 할당
--- 

- 주 메모리는 두 부분으로 구분됨
	- 상주 운영체제용
		- 인터럽트 벡터를 포함하는 위치 사용
		- 대개 하위 메모리에 위치
	- 사용자 프로세스용
		- 대개 상위 메모리에 위치
- 프로세스 메모리 할당 방법 :  연속 메모리 할당, 비연속 메모리 할당

### 메모리 맵핑과 보호
- 재배치 레지스터 방법 
	- 메모리 관리 장치(Memory management unit : MMU)
		- 다음 두 레지스터를 사용하여 메모리 주소 맵핑
		- 재배치(relocation) register : 프로세스가 배치된 물리적 주소의 시작 주소 저장
		- 상한(limit) register : 논리 주소(0부터 시작)의 범위를 저장

### 메모리 할당(Memory Allocation)

1. 고정 분할 방식(Fixed partition scheme)
	- 메모리를 미리 몇개의 고정 크기 영역으로 분할함
	- 동일 크기 방식, 다른 크기 방식
2. 가변 분할 방식(Variable partition scheme)
	- 필요한 만큼 할당해줌
	- 운영체제는 할당된 파티션(allocated partitions), 노는 파티션(free partition, holes)에 대한 정보를 가짐
	- 동적 메모리 할당 문제
		1. First-fit : 요청을 수횽할 수 있는 first hole을 할당
		2. best-fit : smallest fit을 할당. 전체 검색해야하므로 오래걸림
		3. worst-fit : 가장 큰 hole을 할당
	
### <font color="red">메모리 단편화(Memory Fragmentation)</font> 

![8 3_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/16d4d29a-6f39-451e-8296-1e6fa7ab32d1)

- 사용할 수 없는 작은 메모리 영역 존재
- 외부 단편화(External Fragmentation)
	- 요청한 크기보다 작은 메모리 블록들만 존재
	- 장점 
		- 전체 메모리 크기의 합이 요청한 크기보다 큰 경우에도, 요청한 크기의 메모리를 할당할 수 있음
	- 단점  
		- 50-percent rule : first-fit 방식은 메모리의 1/3을 낭비함
		- hole을 추적하는 오버헤드가 큼
- 내부 단편화
	- 요청한 것보다 더 큰 메모리 할당
	- 장점
		- 메모리를 고정된 크기의 블록으로 분할 시, hole의 추적이 필요 없음
	- 단점 
		- 요청한것보다 많은 메모리를 할당받아 일부 메모리가 사용되지 않아 낭비됨
	
##### 외부 단편화 방식에 대한 해결책
1. 압축(compaction)
	- 모든 free memory를 하나의 큰 블록으로 합쳐서 재배치
	- relocation이 동적이고, 실행 시간에 수행될 때 가능
2. 비연속 물리 메모리 공간 허용
	1. 페이징
	2. 세그멘테이션