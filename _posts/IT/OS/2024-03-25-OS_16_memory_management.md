---
title: "운영체제 8장 - 메모리 관리 : 배경지식"
date: 2024-03-25 13:28:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [memory, address, binding, loader, linker]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://cdn.educba.com/academy/wp-content/uploads/2023/02/Flowchart.png"
    alt: 
---
목차
---
- 8.1 배경 지식
- 8.2 스와핑(Swapping)
- 8.3 연속(Contiguous) 메모리 할당
- 8.5 페이징(Paging)
- 8.6 Page Table 구조
- 8.4 Segmentation
- 8.7 Example - Intel IA-32 (Pentium)

#  8.1 배경 지식
---

- CPU가 직접 접근 가능한 기억장치는 CPU register와 main memory
	- CPU register는 1 CPU Clock만에 접근 가능
	- main memory는 여러 CPU Cycle 필요
		- Cache 메모리를 이용해 processor stall 빈도를 줄임

### 메모리 보호(Protection)
- 프로세스에게 독립된 주소 공간을 제공 
- 메모리 보호를 위해 하드웨어 지원이 필요. 운영체제는 이를 관리함

![8 1_0](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ed7655f8-8f7b-487a-ae51-547bd40bb896)

- Base register, limit register 이용
	- 운영체제가 Base register(시작 주소) + limit register(크기) 까지 할당해줌
	- 자신에게 할당된 주소가 아닌경우 trap을 발생시켜 메모리 보호
- 운영체제는 메모리 영역 접근 제한이  없음
	- 특권 명령어로 base, limit 값 적재

### 주소 바인딩(Address Binding)

![8 1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/86de9563-a839-4489-8ca7-378603c5f6cb)

- <font color="red">주소 바인딩</font> 
	- <font color="red">프로그램의 명령어/데이터 주소(Logical address)를 물리적인 메모리 주소(physical address)로 바인딩 하는 것</font>
	- 한 주소 공간에서 다른 주소 공간으로 매핑하는 것
- 링커와 로더
	- inker : 모듈을 함께 묶어서 실행파일을 생성하는 프로그램
	- loader : 프로그램을 메모리에 적재하는 프로그램

![8 1_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/671da1aa-cece-45c1-b4f3-21ffd828daca)

- 사용자 프로그램의 다단계 처리 과정
	- <font color="red">전처리기 컴파일러 어셈블러 링커</font>

```
1. 전처리기가 #include, #define과 같은 전처리 과정 수행
2. 소스 코드를 컴파일러/어셈블러가 obj 코드로 변환
3. 링커가 참조해야할 함수와 라이브러리를 모아 하나의 실행파일로 만들어줌
4. 실행 파일을 로더가 메인 메모리에 적재
```

- 적재 이후 <font color="red">Load Fetch Decode Execution</font> 과정을 거침

### 주소 바인딩 시점
- 정적 바인딩 
	- 초기 운영체제에서 사용하던 바인딩 방법
	1. Compile Time 
		- 컴파일 시점에서, 프로그램이 적재 될 메모리 위치를 미리 알 때
		- 절대 코드(absolute code)를 생성
		- 적재 시작 위치가 변경되면, 코드를 다시 컴파일 해야하는 문제점
	2. Load time 
		- 컴파일 시점에서, 적재될 위치를 실행 전까지 알 수 없을 때
		- 재배치 가능 코드(relocatable code) 생성
		- 주소 바인딩은 적재시 결정
- 동적 바인딩
	- 현대 멀티태스킹 지원 운영체제에서 사용하는 바인딩 방법
	1. Execution time
		- 프로세스가 실행 도중 메모리 위치를 이동할 수 있다면
		- 주소 바인딩을 runtime에 수행함
			- 주소 맵핑을 위한 하드웨어 메모리 관리 장치(MMU) 필요

### 논리 주소와 물리 주소 공간
- 논리 주소 공간 : 프로그램이 사용하는 주소
- 물리 주소 공간 : 물리적 메모리 장치가 사용하는 주소
- 메모리 관리 장치(MMU)
	- 논리 주소를 물리주소로 맵핑하는 작업을 실행 시간에 수행
	- 프로그램은 논리 주소만 알면 되어, 실제 물리적 주소를 알지 못함
- 컴파일 , 적재 시점 바인딩은 논리주소와 물리주소 공간이 같음
- 실행 시점 바인딩은 논리주소와 물리주소 공간이 다름

### 동적 적재(Dynamic Loading)
- 실제 호출되기 전에는 각 루틴은 메모리로 적재되지 않음
- 호출 될 때, 메모리에 적재되어 있지 않다면 relocatable linking loader를 호출하여 요구하는 루틴을 메모리에 적재 후 실행
- 장점 
	- 향상된 메모리 공간 활용 
	- 오류 처리코드와 같이 자주 사용하지 않는 코드의 양이 많은 경우 유용함
- 단점 
	- 프로그래머가 동적 적재를 위한 설계를 책임

### 동적 링크와 공유 라이브러리
- 정적 링크 : linkage editor에 의해 library가 실행 이미지에 링크됨
- 동적 링크 : library의 링크가 실행 시간까지 미루어짐
- Stub
	- 라이브러리를 어떻게 찾는지 알려주는 작은 코드
	- 라이브러리 호출 시, stub코드가 가장 먼저 호출됨
		- 라이브러리 루틴이 메모리에 존재하지 않으면 디스크에서 적재함
		- stub는 실제 library 함수의 주소로 대체되어 다음 호출시에 library함수를 직접 호출하여 실행함
- 공유 라이브러리(shared library)
	- 공유 라이브러리를 사용하는 모든 프로세스는 한 개의 library 코드만 공유하여 사용함
- 동적 적재와 다르게 동적 링크는 운영체제 지원 필요
