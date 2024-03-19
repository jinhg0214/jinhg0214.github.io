---
title: "운영체제 5장 - 프로세서 스케줄링"
date: 2024-03-19 13:00:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [scheduling, processor, multiprocessor, windows, linux, solaris]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://videocdn.geeksforgeeks.org/geeksforgeeks/MultipleProcesserSchedulinginOperatingSystem/MultipleProcesserSchedulinginOS20221022150504.jpg"
    alt: 
---

# 5.4 Thread 스케줄링
---

- 운영체제는 Kernel-level Thread들을 스케줄링
- user-level 스케줄링은 thread library에 의해 수행
- 스케줄링 경쟁 범위(contention scope)
	- 프로세스 경쟁범위(Process-contention scope : PCS)
		- user-level 스레드를 가용 LWP 상에 스케줄링
		- 같은 process의 thread간에 스케줄링 경쟁
		- many-to-one 또는 many-to-many 간에 스케줄링 경쟁
	- 시스템 경쟁 범위(System-contention scope :SCS)
		- 커널이 kernel-level 스레드 들을 CPU 스케줄링
		- system의 모든 thread간에 스케줄링 경쟁
		- one-to-one 맵핑 모델에서 삭용
- Pthread 스케줄링 정책 : thread 생성 시에 지정 허용
	- `PTHREAD_SCOPE_PROCESS`, `PTHREAD_SCOPE_SYSTEM` 

# 5.5 다중 프로세서 스케줄링
---

- 여러개의 CPU가 있는 경우에는 CPU 스케줄링이 더 복잡해짐
- 여기서는 모든 프로세서가 동일한(homogeneous) 시스템을 가정
- 다중 프로세서 스케줄링 접근 방법
	- 비대칭(Asymmetric) 다중 처리
		- 한 프로세서가 스케줄링, 입출력처리, 시스템 활동을 처리(운영체제 커널 수행)
		- 나머지 프로세서들은 user code만 수행
		- 자료 공유 필요성을 배제하므로 간단한 설계
	- 대칭(Symmetric) 다중 처리
		- 각 프로세서는 독자적으로 스케줄링(self-scheduling)
		- ready queue는 공동큐, 분리큐 방식
			- 현대 OS는 프로세서마다 분리된 자신의 큐를 사용하는 분리 큐 방식을 사용함
- 프로세서 친화성(Affinity)
	- 프로세스가 현재 실행중인 프로세서에서 다른 프로세서로의 이주(migration)를 피하고, 다음 스케줄링에서도 현재 프로세서에서 계속 실행을 시도하는 것
	- 프로세서를 이동하면 캐시 무효화와 채우기를 해야하므로 비용이 증가한다
	- 프로세서 친화성 형태
		- 연성 친화성(soft affinity) : 프로세서 지정하지 않음. 이주 가능
		- 강성 친화성(hard affinity) : 프로세서(집합)를 지정
		- 시스템의 형태(특히 주 메모리 구조)가 프로세서 친화성에 영향을 줌
- 부하 균등화(Load Balancing)
	- 모든 프로세서들간에 부하가 고르게 배분하려는 시도
	- 공통큐는 부하 균등화 방식이 불필요함
	- 분리큐를 가지는 시스템의 부하 균등화 방식
		- push migration
			- 특정 task가 주기적으로 각 프로세서의 부하를 검사
			- 과부하 프로세서가 발견되면 process들을 idle 또는 less-busy 프로세서로 이주
		- pull migration
			- idle processor가 busy process에서 대기중인 프로세스를 자신에게로 이주시킴
		- 두 방식은 배타적 방식이 아니므로, 함께 사요할 수 있음
	- 부하 균등화는 프로세서 친화도의 장점과 상충됨
- 멀티스레드 프로세서
	- 하나의 물리적 프로세서에 다수의 논리적 프로세서를 제공하는 프로세서
		- 연산장치를 공유함
	- 논리적 프로세서는 별도의 레지스터 집합을 가져, 빠른 context switching 가능
	- 메모리 접근 동안 연산장치를 다른 논리 프로세서가 이용함
- 멀티코어 프로세서
	- 같은 칩에 다수의 프로세서를 보유한 프로세서
	- 멀티 코어 프로세서의 스케줄링
		- 운영체제 : thread에게 논리 프로세서를 스케줄링
		- CPU 하드웨어 : hardware thread에게 core를 스케줄링

# 5.6 실시간 스케줄링
---

# 5.7 운영체제 사례
---

### Linux 스케줄링 
- kernel version 2.5 이전에는 표준 UNIX 스케줄링 알고리즘의 변형을 사용
- kernel version 2.5/2.6 : 
	- 상수시간에 실행. 
	- Epoch 기반 스케줄링 
	- 모든 runnable tasks는 per-CPU runqueue 자료구조로 관리
	- interactive process의 응답시간이 느림
- 2.6.23+
	- Completely Fair Scheduler(CFS) : 완전 공평 스케줄러
		- 각 task는 특정 우선순위를 가진다
		- 스케줄러는 가장 높은 클래스에서 가장 우선순위가 높은 task 선택
		- 고정된 양의 time  quantum 대신, 우선순위 값에 따라 CPU 시간의 비율을 결정
		- CFS 스케줄러는 task 별로 가상실행시간으 로간리

### Windows 스케줄링
- a priorty-based, preemptive 스케줄링
- real-time, high, above normal, normal, below normal, idle priorty 등의 우선순위를 가짐
- time quantum이 만료되면 우선순위가 낮아짐
- wait이 해제되면 우선순위르 ㄹ높여준다
- foreground 프로세스가 background 프로세스보다 더 높은 우선순위를 가진다

### Solaris 스케줄링
- 우선순위 기반 스케줄링
- 6개의 스케줄링 클래스
	- time sharing, interactive, realtime, system, fiar share, fiexed priory
	- 각 클래스마다 다른 우선순위, 다른 스케줄링 알고리즘 존재

# 5.8 스케줄링 알고리즘 평가
---

- 평가 방법
	- 결정론적(Deterministic) 모델 : 특정한 미리 정의된 부하 사용
	- Queueing 모델 : 부하르 ㄹ확률 분포로 기술. Queueing network의 수학적 분석
	- 시뮬레이션 : 시스템 모델을 프로그래밍하는 작업 포함
	- 구현 : 실제 부하를 사용