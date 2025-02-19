---
title: "운영체제 1장 - Interrupt"
date: 2024-03-04 22:40:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [interrupt]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://www.advancedetiquette.com/wp-content/uploads/2018/10/Interrupt-Image.bmp"
    alt: 
---

# 1.2 Computer-System Operation
---

![1_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/89cf5788-9e64-491e-95b5-3f122303bce4)

- 현대 컴퓨터 구조
	- CPU, 메모리, 버스와 입출력장치들로 구성됨

### Computer System Operation
- <font color="red">Bootstrap program</font>
	- 운영체제를 시작하기 위해 실행되는 초기 시작 프로그램 
	- ROM 또는 EEPROM에 기록되어있으며 Firmware로 불림 
	- 달리기 전에 신발끈을 묶는다 해서 신발끈(Bootstrap)이라는 말이 있음
	
	![1 2 1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/6cd45abc-fb6e-49c6-9bbb-1fefb6b02210)

	> 1. 시스템을 초기화 한 뒤, 0번 블럭의 부트 스트랩 호출
	> 2. 부트 스트랩은 운영체제를 메모리에 적재하고 실행함
	> 3. 운영체제는 메모리에 적재되어 실행됨

### Interrupts
- 인터럽트 : 즉각적인 조치가 필요하다고 CPU에게 알리는 하드웨어 또는 소프트웨어로부터 발생되는 신호
- 인터럽트 타입
	- 하드웨어 인터럽트 : 외부 I/O 디바이스로부터 아무때나 발생
	- 인터널 인터럽트(trap, execpetion) : 실행 에러 등 CPU 내부 오류(0으로 나눔, 메모리 접근 불가, segmentation 오류 등) 
	- 소프트웨어 인터럽트 : OS로부터 발생한 오류 
- 운영체제는 보통 인터럽트가 발생하면 일함
	- 하드웨어 인터럽트 발생 -> 처리
	- 파워 ON, reset도 인터럽트 중 하나임
- 인터럽트의 과정

	![1_2 interrput](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/411351fd-56ad-402f-9b4e-ac8dd11734ad)

	>1. CPU가 인터럽트를 받으면, 하던일을 멈추고 중단된 명령의 주소를Interrupt Instruction에 주소를 저장해둔다
	>2. 인터럽트 서비스 루틴(ISR)에 제어권을 전달함
	>3. 인터럽트가 처리 완료되면, 제어권을 저장된 명령의 주소로 다시 넘겨줌
	>4. 중단된 컴퓨팅을 재개함

- ISR의 주소 결정 방법
	- Polled Interrupt
		- 일반적인 Polling 루틴을 실행하여 일일히 인터럽트 정보를 확인
		- 발생 원인을 찾아야 하므로 느림
	- Vectored Interrupt
		- 인터럽트 요청과 함께 고유번호가 부여됨
		- ISR의 주소는 인터럽트 벡터(주소 배열)에 의해 제공되며, 고유 번호로 인덱싱됨.
		- Polled Interrupt보다 빠른 처리가 가능함
		- 인덱싱 배열만큼 메모리가 더 필요하다

- 인터럽트 타임라인
![1 2 1_int](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/8566b690-ddca-4924-ae5f-a5a5c43fe52a)


### 1.2.2 저장장치 구조
- Main Memory. 주기억장치
	- CPU가 직접 접근 가능한 주기억 장치
	- CPU는 메인 메모리로부터만 명령어를 적재할 수 있음
		- 즉, 프로그램은 실행되기 위해서는 메인 메모리에 적재되어야함!
	- DRAM(Dynamic RAM): 가장 많이 사용되는 램
		- 휘발성이고 크기가 작기때문에, 프로그램은 상주할 수 없음 
- 2차 저장장치
	- 대용량. 비휘발성
	- HDD, SSD와 같은 메모리들
	- disk controller가 논리주소를 결정함
- 캐싱(Caching)
	- 빠른 메모리로 데이터를 복사해 두었다가, 이를 재사용함
	- 저장장치 구조 
	![1 2 2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/088307fd-8293-4dfa-b490-7625052017a3)


### 1.2.3 I/O 구조
- CPU와 입출력 장치 사이는 Device Controller가 담당함
- 각 디바이스 컨트롤러(장치 관리자)는 특정 타입의 디바이스들을 관리함
- I/O Operation을 동작하기 위해서는
 > 1. 장치 관리자가 장치 컨트롤러 내에서 적절한 레지스더를 로드함
 > 2. 장치 관리자는 레지스터를 검사하여 어떤 동작을 수행할지 결정
 > 3.  컨트롤러가 전송을 시작
 
 - I/O Operation의 모드들
	 - Programmed I/O(Polling) 
		 - CPU가 I/O가 처리될때까지 대기함
		 - 입출력이 빨리 처리될 때는 좋지만, 지연되면 문제발생
	 - Interrupt-driven I/O
		 - I/O처리 중에도 다른 동작 수행 가능
		 - I/O 장치가 디바이스 드라이버에 적재가 완료되면, 인터럽트를 통해 동작 완료를 알림. 이후 CPU가 이를 처리함
		 - 적은 양의 데이터를 처리할때 좋음. 많은 인터럽트가 발생할 수 있음
	 - Direct Memory Access(DMA) based I/O
		 - DMA Controller가 CPU의 개입 없이 전송함
		 - CPU는 시작과 끝만, 단순 반복은 DMA가 담당함
			 - 버퍼위치 시작주소 개수 등을 알려줌
		