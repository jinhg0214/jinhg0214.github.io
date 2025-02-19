---
title: "운영체제 11장 - 파일 시스템 구현"
date: 2024-04-01 15:57:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [file, directory, volume, filesystem, allocation, freespace, recovery]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter12/12_03_FileSystemStructures.jpg"
    alt: 
---

## 목차
---

- 11.1 File-System 구조
- 11.2 File-System 구현
- 11.3 Directory 구현
- 11.4 Allocation 방법
- 11.5 Free-Space 관리
- 11.6 효율(Efficiency)과 성능(Performance)
- 11.7 Recovery

## 11.1 File-System 구조
---

- 파일 시스템은 보조 저장장치(디스크)에 위치함
	- 블록 단위 전송 -> I/O 효율 향상
	- 파일 시스템을 통해 디스크에 대한 효율적, 편리적 접근 제공

### 계층적 파일 시스템(Layerd File System)

![11 1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/6389b9af-f1d8-4323-94d0-54d885fa2306)

1. 응용 프로그램(application programs)
2. Logical File System
	- 메타데이터 정보 관리, 심볼릭 파일 이름, 디렉터리 구조 FBC 등 
3. File-Organization module : 
	- logical block을 physical 블록으로 변환. file 공간 관리자
4. basic file system
	- 디스크 드라이버에 명령어 제공. 메모리 버퍼, 캐시 관리
5. I/O control
	- device driver, interrupt handler, 메모리와 디스크 간의 데이터 전송. 직접적인 하드 디스크 전송
6. device
   
- 파일 시스템 종류
	- 대부분의 운영체제가 2개 이상의 파일 시스템 지원
	- 이동 가능한 파일 시스템 - CD/DVD 등
	- 디스크 기반
		- UNIX - FFS
		- Window - FAT, FAT32, NTFS 등
		- LINUX - ext2, ext3, ext4
	- 분산 파일 시스템 : 파일 서버에 있는 파일 시스템을    
	  네트워크로 연결된 클라이언트 컴퓨터에서 마운트 하여 사용함
	  
## 11.2 File-System 구현
---
- 파일 시스템 구현을 위한 자료 구조 
	- 크게 On-disk 구조와 In-Memory 구조로 나뉨
1. On-disk 구조 
	1. 부트 제어 블럭(boot control block)(per 파티션 / 볼륨)
	2. 볼륨 제어 블럭(volume control block)(per 파티션 / 볼류)
	3. 디렉터리 구조(per 파일 시스템)
	4. 파일 제어 블럭(per 파일)
2. In-Memory 구조 
	1. In-Memory Mount Table(partition table) : 마운트된 파티션에 대한 정보
	2. In-Memory directory structure : 최근 접근한 디렉터리에 대한 정보
	3. System-Wide Open table : 현재 오픈된 파일 정보들에 대한 정보
	4. Per-process open table

![11 2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/84e3ae7f-5248-492b-a1ef-b1a7ab13dd5a)

- In-Memory File System 구조

```
(a)Open시 과정
1. File 이름을 디렉터리 구조 내의 전체 시스템 오픈 테이블에 열려있는지 확인한다
2. 존재하지 않는다면, 열려있지 않은 것 이므로, 디렉터리 구조를 찾아 파일 컨트롤 블럭(FCB)를 찾는다. 
3. 이 파일 컨트롤 블록을 전체 시스템 오픈 테이블에 복사한다
4. 프로세스별 오픈 테이블에, 전체 시스템 오픈 테이블에 저장된 파일 컨트롤 블록의 포인터를 저장한다

(b)Read시 과정
1. 프로세스별 오픈 파일 테이블을 찾는다
2. 포인터를 따라 전체 오픈 파일 테이블에 저장된 파일 컨트롤 블록을 참조한다
3. 데이터를 찾아 basic File System이 관리하는 buffer 캐시에 복사한다
4. 프로그램 버퍼에 복사한다
```
### File Control Block
- 파일에 정보로 구성되는 storage structure

![11 2_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f3800730-aa2b-4cc8-b7e0-c3e850efc54a)

### Partitions과 Mounting
- partition은 "raw"(unformatted) 또는 "cooked"(formatted)일 수 있음
- Mounting 
	- root partition이 부트 시간에 마우늩 됨
	- 다른 파티션들은 부트 시간에 자동적으로 마운트 되거나, 나중에 수동으로 마운트 가능
	- 마운팅 정보는 in memory mount  table에 저장됨

## 11.3 Directory 구현
---
1. Linear List
	- 디렉터리를 linear list로 구현
	- 구현하기 쉽지만, 디렉터리 검색에 시간이 소요됨
2.  Hash Table
	- 파일 이름에 대한 Hash 함수 값을 계산하여 Linear list의 위치를 결정
	- 디렉터리 검색 시간이 감소하지만, Hash 충돌에 대한 처리가 필요
	  
## 11.4 Allocation 방법
---
- 디스크 공간을 파일에 할당 할 때, 디스크 공간의 효율적 이용과, 파일의 빠른 접근을 고려
### 3가지 주요 할당 방법
##### 1. 연속 할당(Contiguous Allocation)

<img width="532" alt="11 4_1" src="https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/3c2cfb01-b60b-4853-8ddc-b9f5ba8ba631">

- 각 파일은 디스크의 연속된 블록들의 집합을 점유함
- 장점
	- 최소 탐색 시간
	- 구현이 간단함. 디스크에 시작위치와 크기만 저장하면 됨
	- 임의 액세스 가능
- 단점 
	- 외부 단편화 발생
	- 파일크기 증가 불가능
		- 해결책
			- 사전 할당 -> 내부 단편화 발생
			- 이동 -> 속도 저하
			
##### 2. 연결 할당(Linked Allocation)

<img width="532" alt="11 4_2" src="https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/5dbf26d5-fbb9-43e5-b92a-74c2e32a9420">

- 각 파일은 디스크 블록들의 Linked List로 구성
- 장점
	- 구현이 간단함. 시작위치, 마지막 위치만 디렉토리에 저장
	- 공간 낭비 없음
- 단점 
	- 임의 접근 불가
	- 포인터를 위한 추가 공간 필요 
		- 해결책 : Cluster 단위 할당
- FAT(File Allocation Table) 방식
	- MS DOS에서 사용하는 디스크 공간 할당 방식
	- 포인터들을 파티션의 시작 위치에 저장. 메모리에 캐시되어 사용. 효율적인 접근 가능

##### 3. 인덱스 할당(Indexed Allocation)

<img width="572" alt="11 4_3" src="https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/706ba454-ebfb-4622-9f9c-735b52bd8442">

- 디스크 블록의 포인터를 저장하는 디스크 블록
- 장점 
	- 임의 접근 가능
	- 외부 단편화 발생하지 않음
	- 단점
		- Index 블록 필요
		- 파일 크기 제한
			- 크기 제한의 해결책 - 여러개의 index block을 사용. 
				- linked scheme
				- multilevel index
				- combined scheme 
					  
## 11.5 Free-Space 관리
---
- Bit vector 방식
	- free space list를 bit map 또는 bit vector로 구현
	- 장점
		- 비교적 간단함. 첫째 free  bock 및 n개의 연속 free block을 찾기 쉬움
	- 단점 
		- bit map은 추가 공간이 필요함
		- 전체 bit vector를 메모리에 적재할 수 없으면 비효율적임
- Linked list (free-list)
	- 장점 : 공간 낭비가 없음
	- 단점 : 연속 공간을 쉽게 얻을 수 없음
- Grouping
	- free list 방식의 수정
	- 첫번째 free block에 n 개의 free block 의 주소를 저장함
- Counting
	- free list는 연속된 free block의 첫번째 주소와 블록의 개수를 저장함
	   
## 11.6 효율(Efficiency)과 성능(Performance)
---
- 효율(Efficiency)
	- 디스크 할당 및 디렉터리 알고리즘이 디스크 공간의 효율적 사용에 영향을 줌
	- 디렉터리 entry에 저장되는 데이터 종류를 고려
- 성능(performance)
	- disk cache
		- 빈번히 사용되는 disk block을 저장하는 메모리 부분
		- buffer cache, page cache
	- free-behind 및 read-ahead
		- 순차 접근을 최적화 하기 위한 기법

## 11.7 Recovery
---
- 파일과 디렉터리는 주메모리와 디스크에 모두 저장됨
	- system failure는 데이터 손실 및 데이터 비일관성(inconsistency)을 가져올 수 있음
- 일관성 검사(Consistency Check)
	- 디렉터리 구조에 있는 데이터와 디스크의 데이터 블록을 비교하여 비일관성을 검사
- Backup
	- 시스템 프로그램을 사용하여 디스크 데이터를 다른 보조저장장치로 백업
- Restore
	- 백업 장치로부터 데이터를 복원하여 손실된 파일/디스크를 복구

### 로그 구조(Log Structured) 파일 시스템

- 로그 고자 파일 시스템(Journaling 파일 시스템)
	- 파일에 대한 각 update를 transcation으로 기록함
- 모든 트랜잭션은 로그에 기록됨
- 로그에 있는 트랜잭션은 비동기적 파일 시스템에 기록됨
- 파일 시스템에 기록되는 동안 시스템이 고장나면, 로그에 있는 모든 남아있는 트랜잭션은 다시 수행될 수 있음
- 로그에 기록되는 동안 시스템이 고장나면, 해당 트랜잭션은 실행되지 않음
	- 파일 시스템 일관성은 유지됨