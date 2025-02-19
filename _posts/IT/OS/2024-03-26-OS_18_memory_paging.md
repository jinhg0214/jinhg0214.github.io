---
title: "운영체제 8장 - 메모리 관리 : 페이징과 세그먼테이션"
date: 2024-03-26 14:55:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [memory, paging, segment, pagetable]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://www.onlinenotesnepal.com/storage/topics/stZUePfnMPRy3aysZK2EpvARxG1ATOTqx6O8L4Ah.png"
    alt: 
---

# 8.5 페이징(Paging)

---

- Paging
	- 비연속(noncontiguous) 물리적 메모리 할당 방법 중 하나
	- 고정 방식은 paging, 가변 방식은 segment
	- page frame : 물리적 메모리를 고정된 크기의 블록(2^n, 대개 512B ~ 8KB)으로 나눔
	- page :  논리 메모리를 같은 크기의 블록으로 나눔
	- page를 page frame에 맵핑하는 방식

### 주소 변환 방식

![8 5_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/0ed42857-bea5-4f81-b08f-3d8cdadbaa3b)
- 상위 부분 : Page number
- 하위 부분 : page 내에서의 offset
- 주소 변환
	- 논리 주소의 page number를 물리 주소의 page frame nubmer 로 변환
	- 주소 변환에 page table을 사용

### Page Table
- 각 페이지의 시작 주소들을 저장한 물리적 메모리에 저장된 테이블
- page number가 index로 사용됨. 각 프로세스마다 하나씩 가진다
![8 5_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/5b9e8f1a-4b98-4e13-8fb0-420987d96cd8)

### Page 할당 방법
![8 5_3](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/fe8038dc-026d-455c-85ee-4528b77f5284)

- Free frame list를 추적. page 할당 후 적재
- page table에 page frame 번호 설정
- 메모리 할당 단위 : page frame(고정 크기)
	- 외부 단편화는 없으나, 내부 단편화 발생
	- 페이지 프레임이 작으면 내부 단편화는 적지만,    
	  많은 수의 페이지로 인해 큰 테이블 페이지 필요.    
	  반대로 페이지 프레임이 크면 테이블 페이지는 작지만,   
	  내부 단편화가 큼
	- 일부 CPU와 커널은 복수의 페이지 크기를 지원하기도 함

### Frame Table과 Page Table
- Frame Table 
	- 각 프레임당 하나씩(할당 되었는지, 누구에게 등에 대한 정보)
- 운영체제는 각 프로세스마다 자신의 페이지 테이블을 가짐
- Page Table 구조
	1. 전용 레지스터로 구현 : 크기가 작을때만 사용 가능
	2. 주 메모리에 유지
		- page-table base Register :PTBR
		- page-table length Register : PTLR 
		- 총 두번의 메모리 접근 필요. 이 문제를 해결하기 위해 TLAR을 사용함
- Translation Look-aside Register(TLAR) 
	- page table entry용 캐시 사용
	- 페이지 테이블의 내용을 캐시에 적재

### 메모리 보호
- Page Table Extry에 Protection bit, valid bit 추가
	- protection bit : RW/R/E 등의 정보
	- valid bit : 1 유효, 0 무효
- 공유 페이지
	- 페이지는 쉽게 공유 가능
	- 공유 코드 
		- 재진입 코드는 공유될 수 있음
			- 재진입 코드 : 수행하는 동안 변하지 않는 읽기 전용 코드

# 8.6 Page Table 구조
---
- Page Table 크기
	- 2^32 ~ 2^64의 커다란 논리 주소 공간을 사용
	- 연속 메모리 공간을 사용하는 page table은 page size보다 큼
	- 이를 해결하기 위해 계층적 테이블 구조, 해시 페이지 테이블 등을 사용함
- 페이지 테이블 구조
	1. 계층적 테이블 구조(Hierarchical Page Table)  

		![8 6_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/3b8296c3-4532-4265-87fd-114248e89dd5)
		
		- 다단계 page table
		- page table을 하나의 연속적 메모리를 사용하지 않고,    
		  여러개의 작은 조각으로 나누어서 계층적으로 구현
			- Two-Level, Multi-Level 등으로 나뉨
		- Page Frame보다 table 이 큰 경우 : 비연속 메모리 할당
		- 하나의 연속적 메모리를 사용하지 않고, 여러개의 작은 조각으로 나누어 구현
		- page directory -> page table -> f + d
		- 단점 : 접근 시간이 LuL이 높아질수록 느려짐
	2. 해시 페이지 테이블(hashed page table)

		![8 6_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/137a0d1a-d390-4da7-9347-ee72e49ad906)

		- 주소 공간이 32비트보다 커지면 hashed page table을 사용
		- 논리 주소의 page number의 hash function 값을 hased page table의 index로 사용함
		- hash table의 각 entry는 연결 리스트를 저장함
		  
# 8.4 Segmentation
---

![8 4_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/88975dc1-eca0-497d-b3d1-ad3785a97d9d)

- 가상 메모리를 서로 크기가 다른 논리적 단위로 분할한 것
- 프로세스를 물리적 단위인 페이지가 아닌,   
  논리적 단위인 세그먼트로 분할해서 메모리에 적재하는 방식
- 프로그램은 세그먼트의 집합
	- 세그먼트는 프로그램의 논리적 단위
- 세그먼테이션(segmentation)
	- 논리 주소 = `<segment number, offset>`의 2차원 주소를    
	  1차원 물리 주소로 변환
- 장점
	- 내부 단편화 문제가 해소
	- 보호와 공유 기능을 수행. 
	- 프로그램의 중요한 부분과 중요하지 않은 부분을 분리하여 저장할 수 있고, 같은 코드 영역은 한 번에 저장할 수 있다.
- 단점
	- 외부 단편화 문제

### Segmentation 구조
- Segment Table
	- 프로그램의 각 세그먼트 주소 변환 정보 저장
	- base : 세그먼트의 물리적 시작 주소
	- limit : 세그먼트의 길이
		- 가변적이므로 동적 할당 방법 사용
- Segment-Table base Register : STBR
	- 현재 프로세스의 segment table의 시작 주소
- Segment-Table Length Resgiser : STLR
	- 현재 프로세스의 segment 개수
  
# 8.7 Example - Intel IA-32 (Pentium)
---

![8 7](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/73b6f3bf-9a43-432f-a730-2268ce933f1a)

- Segmentation과 Segmentation With Paging 을 지원함
- Page Extension Address(PAE) 사용
- 앞 두비트를 page table로 사용하여 1단계 index 4kb, 2단계 4gb, 3단계 64gb로 사용함