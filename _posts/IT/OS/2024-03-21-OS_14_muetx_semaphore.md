---
title: "운영체제 6장 - 프로세스 동기화"
date: 2024-03-21 11:38:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [semaphore, mutex, critical-section]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://techdifferences.com/wp-content/uploads/2016/12/Semaphore-Vs-Mutex.jpg"
    alt: 
---

목차
---

6.1 배경

6.2 임계구역 문제(Critical-Section)   

6.3 Peterson의 해결안   

6.4 동기화 하드웨어(Synchronization Hardware)   

6.5 뮤텍스 락(Mutex Locks)   

6.6 세마포어(Semaphores)   

6.7 고전적인 동기화 문제   

6.8 모니터(Monitors)   

6.9 동기화 사례   


# 6.1 배경
---
- 협력 프로세스(Cooperating Process) : 다른 프로세스의 실행을 영향을 주거나 받는 프로세스
	- 직접 메모리를 공유하거나 파일 또는 메시지를 간접적으로 공유할 수 있음 
- 공유데이터에 대한 병행/병렬 접근은 데이터의 일관성을 보장하지 못함
- 경쟁 조건(Race condition)
	- 여러개의 프로세스가 공유자료를 접근하여 조작하고, 그 실행 결과가 자료 접근 순서에 영향을 받는 상황
	- 둘 이상의 입력 조작이 동시에 이루어지는 상태

# 6.2 임계구역 문제(Critical-Section)
---
- 임계 구역
	- 프로세스가 공유 자원을 변경할 수 잇는 부분
- 임계구역 문제
	- 프로세스들이 임계구역에서 경쟁 조건이 발생하지 않도록, 서로 협력하기 위한 규약 설계
	- 임계구역의 해결책 3가지 필요 조건
		1. 상호 배제(Mutual Exclusion) : 누군가 쓰고있으면, 다른 프로세스는 진입 불가
		2. 진행(Progress) : 임계구역에 프로세스가 없다면 어떠한 프로세스라도 들어가서 자원을 활용할 수 있다
		3. 한정대기 : 상호 배제 때문에 기다리게 되는 프로세스가 무한 대기하지 않아야 한다.
- 커널에서의 경쟁조건과 선점/비선점 커널
	- 선점형 커널 : 커널모드에서 프로세스가 선점되는 것을 허용, 경쟁조건이 발생하지 않도록 설계
		- 대칭 다중처리에서는 어려움
	- 비선점형 커널 : 선점되는것을 비허용함
		- 커널모드 종료
		- 봉쇄
		- 자발적 양보시까지 계속 실행

# 6.3 Peterson의 해결안
---
- Peterson 알고리즘
	- 임계 구역 문제에 대한 고전적인 소프트웨어 기반 해결방안
	- 두 프로세스에 대해서만 적용 가능
- Dekker 알고리즘
- n-프로세스 알고리즘 등
- 알고리즘 방식은 실제로 많이 사용하지 않음 
	- 매우 복잡하고, 정확성에 대한 증명이 복잡하므로
	
# 6.4 동기화 하드웨어(Synchronization Hardware)
---
- 하드웨어적인 도움을 통해 임계구역 문제 해결
- 임계구역을 Lock으로 보호하여, 경쟁 조건을 방지
- 하드웨어적인 Lock의 구현 방법
	1. 인터럽트 금지(interrupt disable) : 임계구역 실행동안 인터럽트를 방지하여, 선점을 비허용함
		- 단일 프로세서 가능, 그러나 다중 프로세서에선 적용 불가
		- 시스템 클럭의 영향을 받음
		- 쉽지만 부작용이 많다
	2. 원자적 명령어(Atomic Instruction) : 나눌 수 없는 명령어를 실행
		- 연속되는 여러개의 동작을 한버에 수행하는 명령어를 통해, Lock 변수를 처리
		- lock 변수 사용하기

# 6.5 뮤텍스 락(Mutex lock)
---
- 운영체제가 지원하는 소프트웨어적 도구
- 상호배제 락
- `aquire()` `release()`를 통해 락을 얻고, 반환함

1. Spin Lock : Busy Waiting 
	- 루프를 반복 실행하면서 lock 획득을 기다림
	- 주로 반복 실행하면서 lock 획득 대기
	- 장점 : 짧은 시간내에 lock 을 얻을 수 있다면 유용함
	- 단점 : CPU 사이클 낭비

2. Busy waiting이 없는, Test And Set을 통한 사용 배제
	- lock 획득 못할 경우, 대기상태로 전환
	- `while(TestAndSet(&lock)){block();}` : 다른 프로세스가 release() 호출하여, unlock되면 wakeup 호출됨

# 6.6 세마포어(Semaphores)
---

- 특별한 표준 동기화 연산을 통해서만 접근할 수 있는 정수 변수
	- 보통 사용 가능한 자원의 수를 나타냄
- 세마포어 연산
	1. 초기화 연산 : `sempahore s`의 값을 초기화
	2. 두개의 원자적인 연산
		- P operation (Proberen = test) : wait(s), aquires(s)
		- V operation (Verhogen = increment) -> signal(s), release(s)
- 세마포어 용도
	1. 상호 배제(Mutual Exclusion) : 이진 세마포(binary semaphore)
		- semaphore 0 or 1, 임계구역 접근 제어에 사용
		- 뮤텍스 락과 유사
	2. 유한한 자원 접근 : 카운팅 세마포(Counting semaphore)
		- 세마포어 값이 가용 자원의 개수
	3. 일반적인 동기화
		- 프로세스 P1의 A부분 실행 후에 프로세스 P2의 B 부분이 실행되어야할 때 사용
 -  세마포어 구현
	 1. 바쁜 대기 : spinlock 방식. 세마포어를 획득할때까지 루프
	 2. 바쁜 대기 없는 세마포어 : 
		 - value : 세마포어 값
		 - list : 이 세마포어를 대기하는 프로세스 리스트를 대기 큐로 가짐
 - 교착 상태
	- 두개 이상의 작업이 서로 상대의 작업이 끝나기를 기다리고 있어, 아무것도 완료되지 못하는 상태
- 기아
	- 일부 프로세스가 무한정 대기하는 상황

# 6.7 고전적인 동기화 문제
---

1. 유한 버퍼 문제(Bounded-Buffer Problem)
	- 생산자가 버퍼에 저장해두면, 소비자가 꺼내감
	- 두 프로세스가 하나의 버퍼라는 자료를 공유함
	- 소비자는 버퍼에 하나 이상의 원소가 존재할 때, 원소를 소비해야 하고, 생산자는 버퍼에 빈 공간이 존재할 때 원소를 생산해야 한다

```c
/*
    producer
*/
do {
    ... // produce item
    
    wait(empty);
    wait(mutex);
    
    ... // add item to buffer
    
    signal(mutex);
    signal(full);
} while(TRUE);
 
/*
    consumer
*/
do {
    wait(full);
    wait(mutex);
    
    ... // remove item
    
    signal(mutex);
    signal(empty);
    
    ... // consume item
} while(TRUE);
```

2. Readers, Writers 문제
	- 여러 프로세스가 하나의 자료를 공유함
	- 일부는 읽기 수행, 일부는 갱신 수행
	- 여러 개의reader만 공유데이터에 접근하는 경우 세마포어를 사용하지 않고도 데이터의 일관성을 유지할 수 있는 반면,    
	  하나 이상의 writer가 개입하는 경우 데이터의 일관성이 깨질 수 있음
	- writer가 아직 공유데이터에 대한 접근할 준비가 되지 않았다면 reader들이 기다리는 시간을 길게해서는 안된다. 이는 프로세서의 자원을 낭비하는 것이기 때문이다.
	- writer가 준비가 되었다면 곧바로 writer가 쓰도록 해 주어야 한다.
	  
```c
/*
    writer
*/
do {
    wait(rw_mutex);
    
    ... // writing
    
    signal(rw_mutex);
} while(TRUE);
 
/*
    reader
*/
do {
    wait(rc_mutex);
    read_count++;
    if(read_count == 1) // 읽기동작을 수행하는 첫 번째 reader인 경우
        wait(rw_mutex); // rw_mutex를 기다린다.
    signal(rc_mutex);
    
    ... // reading
    
    wait(rc_mutex);
    read_count--;
    if(read_count == 0) // 읽기동작을 끝낸 마지막 reader인 경우
        signal(rw_mutex); // rw_mutex를 놓아준다.
    signal(rc_mutex);
} while(TRUE);
```

3. 식사하는 철학자 문제
	- 여러 프로세스가 여러 자료를 공유할때 생기는 문제
	- 5명의 철학자가 각자 왼쪽과 오른쪽에 젓가락이 있을 때, deadlock과 starvation이 발생하지 않으면서, 식사를 할 수 있도록 해야함

```
/*
    process of Pi
*/
do {
    wait(chopstick[i]);
    wait(chopstick[(i+1) % 5]);
    
    ... // 맛잇게 식사
    
    signal(chopstick[i]);
    signal(chopstick[(i+1) % 5]);
    
    ... // 생각
} while(TRUE);
```

- 위 코드는 교착상태의 가능성이 존재. 모두 왼쪽 젓가락을 집으면 오른쪽 젓가락을 집기 위해 무한히 대기
- 이를 위한 해결책 
	- 5개의 자리 중 4명만 앉을 수 있게
	- 철학자 양쪽에 젓가락이 존재할 때만, 임계구역 내에서 젓가락을 집게 하기
	- 짝수번째 철학자는 왼쪽부터, 홀수번째 철학자는 오른쪽부터 집기 등

# 6.8 모니터(Monitors)
---

- 세마포어는 프로그래머가 잘못 사용하면 다양한 오류 발생 가능
	- 이를 방지하기 위해 모니터 사용
		- 모니터 : 프로세스 동기화를 위한 고수준의 동기화 추상화 데이터형
- 모니터에 정의된 procedure만이 monitor 내에 local data 접근 가능
- 모니터의 procedure들은 한번에 하나의 프로세스만 진입하여 실행할 수 있어, 상호배제가 보장됨
- `condition`이라는 모니터에서 동기화용으로 특별한 자료형 사용

# 6.9 동기화 사례
---

- pthread 동기화
	- mutex locks
	- condition variables
	- non-protable extensions 등