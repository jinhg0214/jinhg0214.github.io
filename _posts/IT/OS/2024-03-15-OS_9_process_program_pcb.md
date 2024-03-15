---
title: "운영체제 3장 - 프로세스, 프로그램, 프로세스의 상태와 PCB"
date: 2024-03-15 17:30:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [process, program, pcb, state]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://cdn.educba.com/academy/wp-content/uploads/2020/01/Program-vs-Process.jpg"
    alt: 
---

# 목차
---
3.1 프로세스 개념
3.2 프로세스 스케쥴링 (5장에서 상세히)
3.3 프로세스 연산
3.4 프로세스간 통신
3.5 IPC 시스탐 사례 - UNIX의 공유 메모리 함수
3.6 클라이언트-서버 시스템에서의 통신

# 3.1 프로세스 개념
---

### 프로세스와 프로그램
- 프로세스
	- 운영체제로부터 자원을 할당받은 **작업의 단위**
	- <font color="red">운영체제로부터 메모리 공간을 할당받아 컴퓨터에서 실행중인 프로그램</font>
	- 실행 상태에 있는 능동적 개체, 연관된 자원 사용
		- 메모리, CPU, 프로세스 관리용 자료구조 등

	![3 1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/d055dd78-2011-474a-889c-6cabdf4d7440)
	- Stack: 지역변수와 같은 임시 데이터 저장. 함수의 호출과 함께 할당되며, 호출이 완료되면 소멸함
	- Heap : 실행시간동안에 동적 할당되는 메모리. 생성자, 인스턴스와 같은 동적으로 할당되는 데이터들을 위해 존재하는 공간 
	- Data : 전역변수나 각종 데이터를 저장함 `.data, .BSS. rodata`로 세분화 됨
		- `.data` : 전역변수 또는 static 변수 등 프로그램이 사용하는 데이터를 저장
		- `.BSS` : 초기값 없는 전역변수, static 변수가 저장
		- `.rodata` : const같은 상수 키워드 선언된 변수나 문자열 상수를 저장
	- Text/Code : 프로그래머가 작성한 코드가 기계어 형태로 저장됨

- 프로그램
	- 파일을 실행하지 않은 상태이기 때문에 정적 프로그램(Static Program), 줄여서 프로그램이라 부름
	- 디스크에 파일로 저장된 수동적 개체
		- 프로그램 파일 = 코드(text section) + 초기화 데이터 + 헤더정보 등

- 한개의 프로그램을 여러개의 프로세스로 실행할 수 있음
	- text section은 같음. 같은 메모리 공유, 개별적 실행 시퀸스
	- data, stack, heap section 은 각각 다름. 프로세스마다 개별적인 메모리 할당됨

### 프로세스 상태

- 생성, 실행, 대기, 준비, 종료 5가지 상태를 가지며 변화함
	![3_1_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/66d200e8-f8b1-4328-b1a3-729eccce2776)
	- 생성(new) : 프로세스가 생성중
	- 실행(running) : 명령어가 실행되는 중
	- 대기(waiting) : 어떤 사건(입출력 완료, 신호 수신등) 발생을 기다림
	- 준비(ready) : 프로세스가 실행할 준비가 됨 -> CPU 할당을 기다림
	- 종료(terminated) : 프로세스의 실행이 종료됨 -> 종료 후 처리를 기다림
	- 한 CPU에서는 어떤 순간에 한 프로세스만 실행될 수 있다
	
### 프로세스 제어 블록(PCB)
- Process Control Block
	- 프로세스에 대한 정보를 포함하는 OS 커널의 자료구조
	- 태스크 제어 블록이라고도 함
- 프로세스 Context
	- 프로그램의 실행 상태 정보를 저장
- PCB 정보
	- Process state, Program Counter(PC), CPU registers, Memory-management 정보, CPU scheduling 정보, I/O status 정보, the list of allocated I/O devices, the list of I/O requests. the list of open files … 
- 프로세스 간  CPU Switch
	 ![[Pasted image 20240315172914.png]]
	 - 프로세스 0의 PCB_0 정보를 저장 후, 프로세스 1의 PCB_1를 가져옴
	- 프로세스 0의 차례가 오면 다시 저장후 가져옴
	- 