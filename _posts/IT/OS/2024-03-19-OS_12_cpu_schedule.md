---
title: "운영체제 5장 - CPU 스케줄링 알고리즘들"
date: 2024-03-19 10:00:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [scheduling, preemption, nonpreemption, fcfs, sjf, rr]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://media.geeksforgeeks.org/wp-content/uploads/20220526113439/CPUSchedulingAlgorithmsinOperatingSystems3.jpg"
    alt: 
---

# 목차
---

5.1 기본 개념

5.2 Scheduling 기준(Criteria)

5.3 스케줄링 알고리즘(Scheduling Algorithm)

5.4 Thread 스케줄링

5.5 다중 프로세서 스케줄링

5.6 실시간 스케줄링

5.7 운영체제 사례

# 5.1 기본 개념
---

- 멀티프로그래밍의 목적은 CPU의 이용률을 최대화 하기 위함
- 프로세스 실행 사이클은 CPU 실행과 I/O 대기의 사이클로 구성됨
	- 일부만 CPU 사용률이 높고, I/O는 CPU를 적게 사용함
- CPU Scheduler(short-term scheduler)
	- ready queue에 있는 프로세스들 중 하나를 선택하여 이 프로세스에게 CPU를 할당함
	- CPU 스케줄링 시점
		- 언제 CPU 제어권을 넘겨줘야하는가?
		- 비선점(non-preemptive) 스케줄링 
			- 다음과 같은 경우는반드시 스케줄링하여 새 프로세스를 선택해야함 
				- running -> waiting 상태로 전환
				- terminate시
		- 선점(preemptive) 스케줄링
			- CPU 독점을 방지하거나, 프로세스 우선순위를 반영하고자 할 때 스케줄링
				- running -> raedy 상태로 전환
				- waiting -> ready 상태로 전환
			- 대부분의 현대 OS가 이를 사용함
- 선점 스케줄링의 문제점과 해결책
	- 공유 데이터의 일관성(consistence) 문제
		- 문제 상황 1)    
		  한 프로세스가 데이터를 변경하는 도중에   
		  다른 프로세스에게 선점되어 변경된 데이터를 저장하기 전에   
		  CPU 제어권을 빼앗길 수도 있다
		- 문제 상황 2)    
		  다수의 프로세스가 데이터를 공유할 때,   
		  경쟁적으로 데이터를 변경하면    
		  데이터 일관성이 유지되지 않을 수 있다 -> 경쟁 조건(Race condition)
		- 공유 데이터 접근에 대한 조정이 필요함!
	- user mode에서의 preemption
		- 한 프로세스가 데이터를 변경하는 동안,    
		  다른 프로세스가 같은 데이터를 읽거나 수정하면 일관성이 유지되지 않음
		- 운영체제가 제공하는 동기화 방식을 사용하여 데이터 일관성 문제가 발생하지 않도록 해야함
	- kernel mode에서의 preemption
		- 모든 커널 루틴은 커널 데이터를 공유함
		- 커널은 system call을 통하여 요청된 프로세스의 작업을 처리할 수 있으며,    
		  공유 데이터를 접근하는 커널 루틴이 실행되는 동안에,    
		  인터럽트로 인해 다른 커널 루틴에게 선점되면    
		  공유 데이터 일관성 유지가 되지 않을수 있음
			- 이러한 문제는 시스템 전체에 영향을 줄 수있으므로 매우 위험함!
		- 운영체제 커널에서의 preemption 처리 방법
			- 비선점형 커널 - 커널 내에서의 preemption을 허용하지 않음
				- system call이 완료되거나, I/O block이 발생할때 까지 기다린 후에 context switching 수행
				- 실시간 컴퓨팅을 지원하는데 부적합
			- 선점형 커널 - 커널 내에서 preemtion을 허용
				- 커널 내에서 공유 데이터 접근에 대한 동기화를 사용하여, 커널을 작성해야함
				- 실시간 컴퓨팅을 지원하는데 적합
- 디스패처(Dispatcher)
	- CPU의 제어권을 CPU 스케줄러가 선택한 프로세스에게 주는 모듈
	- context switching, CPU 동작 모드를 user mode로 전환, 선택한 프로세스가 다시 시작하도록, user program의 적절한 위치로 이동 등을 수행함
	- Dispatch 지연 :
		- 한 프로세스를 정지하고, 다른 프로세스의 수행을 시작할 떄 까지 소요되는 시간
		- 가능한 작아야 성능이 좋다

# 5.2 Scheduling 기준(Criteria)
---

- CPU 이용률(Utilization) 
- 처리량(Throughput) : 단위 시간당 수행이 완료된 프로세스 
- 총 처리시간(Turnaround Time) : 프로세스를 실행하는데 소요된 시간
- 대기 시간(Waiting Time) : ready queue에서 대기하는 시간
- 응답 시간(Response Time) : 응답을 받을때까지 소요 시간

# 5.3 스케줄링 알고리즘(Scheduling Algorithm)
---

- 선입선처리(First-Come First-Serve: FCFS) 스케줄링
- 최단작업우선(Shortest-Job-Firest :SJF) 스케줄링
- 우선순위(Priority) 스케줄링
- 라운드로빈(Round Robing:RR) 스케줄링
- 다단계 큐(Multilevel Queue) 스케줄링

##### 선입선처리(First-Come First-Serve: FCFS) 스케줄링
- 먼저 요청한 프로세스가 먼저 자원을 제공 받음
- 이미 사용중이라면, 사용이 끝날때까지 기다림
- 장점 : 
	- 스케줄링이 단순함
	- 모든 프로세스가 실행 가능(기아 X)
	- 프로세서가 지속적으로 유용한 프로세스를 수행하여 처리율이 높음
- 단점 :
	- 비선점방식의 스케줄링이므로 대화형 작업에는 부적합
	- 호위 효과(Convoy Effect) :
		- 긴 프로세스 뒤에 짧은 프로세들이 있는 경우, 짧은 프로세스들의 평균시간이 증가함
##### 최단작업우선(Shortest-Job-Firest :SJF) 스케줄링
- 프로세스의 실행시간을 이용하여 가장 짧은 시간을 갖는 프로세스가 먼저 자원을 할당받음
	- 가장 짧은 시간을 갖는 프로세스를 어떻게 찾는가?
		- 모든 프로세스 실행시간을 갖고 있어야 비교 가능
		- 예측을 통해 가장 짧은 실행시간을 갖는 CPU Burst를 계산함
			- 이전의 기록, 유사한 프로세스 등
- Shrotest Process Next(SPN), Shrotest Request Next(SRN)이라고도 불림
- 두가지 방식의 SJF 스케줄링
	- 비선점 SJF : 프로세스는 CPU burst를 끝낼때 까지 선점되지 않음
	- 선점 SJF : 새 프로세스의 CPU burst가, 현재 프로세스의 잔여 시간보다 작다면, 현재 프로세스가 선점되어 스케줄링됨
- 장점 :
	- 항상 짧은 작업을 먼저 수행하여, 평균 대기시간이 가장 짧음
- 단점 :
	- 수행 시간이 긴 작업은 짧은 작업에 밀려 기아가 발생함
	- 실행시간을 예측하기 어려워 실용적이지 못함
	- 짧은 작업이 먼저 실행되므로 공정하지 못함
##### 우선순위(Priority) 스케줄링
- 프로세스마다 우선순위 속성을 가짐
	- 보통 우선순위가 높을수록 작은 번호를 가짐
	- 우선순위가 같다면, FIFO 방식으로 동작함
- 우선순위 정의에 고려되는 요인들
	- 내부적 : 시간 제한, 메모리 요구, open file수, I/O와  CPU의 비율 등
	- 외부적 :  프로세스 중요성, 비용의 유형과 액수 등
- 두가지 방식의 우선순위 스케줄링. 비선점, 선점
- SJF는 우선순위 스케줄링의 특별한 케이스. CPU burst time을 우선순위로 둔 경우임
- 장점 :
	- 각 프로세스의 중요성에 따라 작업을 수행하므로 합리적
	- 실시간 시스템에서 사용 가능
- 단점 : 
	- 기아상태(starvation) 와 무한 봉쇄(indefinite blocking)
	- 낮은 우선순위를 가지는 프로세스는 무한히 대기할수도 있음
		- 이를 해결하기 위해 노화(aging) 방식을 적용하기도 함
			- 오랫동안 대기하는 프로세스는 우선순위를 점진적으로 증가시킴
##### 라운드로빈(Round Robing:RR) 스케줄링
- 시분할 시스템을 위해 설계된 스케줄링 기법
- Time-Slice, Time quantum이라는 작은양의 시간을 할당
	- 전체 CPU burst time의 80%정도가 quantum time 보다 짧도록 정함
- 실행 프로세스는 이 시간 경과 후 선점되어, ready queue의 끝으로 이동함
- 타이머 인터럽트를 사용
- 장점 : 
	- 모든 프로세스가 공정하게 스케줄링 받을 수 있음
	- 실행 큐에 프로세스의 수를 알고있을 때 유용함
	- 프로세스의 짧은 응답시간을 가지고, 최악의 응답시간을 알 수 있음
	- 평균 대기시간이 FIFO와 SJF보다 적음
- 단점 :
	- 성능은 규정 시간량의 길이에 따라 달라지므로, 너무 길어지면 FIFO로 변하고, 너무 짧으면 Context Switching 비용이 증가함
	- 하드웨어적인 타이머 필요
	- 미완성 작업은 규정 시간량을 마친 후, 프로세서를 기다려야하므로, 평균 처리시간이 높음
##### 다단계 큐(Multilevel Queue) 스케줄링
- Ready queue가 여러개의 큐로 분할됨
- 프로세스가 생성 시 하나의 큐에 영구적으로 할당됨
- 각 큐는 자신의 스케줄링 알고리즘을 각각 사용한다
- 큐 간의 스케줄링도 있음
	- 고정 우선순위 스케줄링, Time slice 등을 사용
- 장점 : 
	- 응답이 빠름
- 단점 :
	- 여러 준비 큐와 스케줄링 알고리즘 때문에 오버헤드 발생
	- 우선순위가 낮은 큐의 프로세스는 무한정 대기하는 기아 발생 가능
##### 다단계 피드백(Multilevel Feedback) 큐 스케줄링
- 프로세스가 여러 큐들 사이의 이동을 허용함
- CPU burst가 크면 서서히 낮은 단계로 이동한다
- 큐 개수, 각 큐에 대한 스케줄링 알고리즘, 큐 승급/강등 시점, 진입할 큐 결정 방법 등의 매개변수 필요
- 장점 :
	- 매우 유연하여 스케줄러를 특정 시스템에 맞게 구성할 수 있음
	- 자동으로 입출력 중심과 프로세서 중심 프로세스를 분류함
	- 적응성이 좋아 프로세스의 사전 정보가 없어도 최소작업 우선 스케줄링의 효과를 보여줌
- 단점 : 
	- 설계와 구현이 매우 복잡함