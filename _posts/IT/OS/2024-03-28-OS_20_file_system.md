---
title: "운영체제 10장 - 파일 시스템"
date: 2024-03-28 16:22:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [file, filesystem, disk, volume, partition, directory]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://cdn.ttgtmedia.com/rms/onlineImages/TT_tree_mobile.jpg"
    alt: 
---

## 목차
---
- 10.1 파일 개념
- 10.2 접근 방법
- 10.3 디렉터리와 디스크 구조
- 10.4 File-System mounting
- 10.5 공유
- 10.6 보호

## 10.1 파일 개념
---

- 파일(File)
	- 운영체제가 정보 저장장치의 물리적 특성을 추상화 한 논리적 저장 장치
	- 파일 구조는 byte의 연속
- 파일 구조
	- 없음 - 바이트 또는 워드의 연속
	- 단순 레코드 구조 - 줄, 고정 길이, 가변 길이 레코드의 연속
	- 복잡한 구조 - formatted 문서, relocatable load file 등
- 운영체제에서는 파일 구조를 byte의 연속으로 다룬다

### 파일 속성 및 연산

- 파일 속성(File attributes)
	- 이름, 식별자(identidier), 유형, 위치, 크기, 보호, 날짜시간, 사용자 등
- 디렉터리 구조
	- 파일 이름을 파일 위치로 변환하는 심볼릭 테이블
	- 보조 저장장치에 저장되어 모든 파일에 대한 정보 유지
	- 파일 이름, 다른 파일 속성을 찾기 위한 식별자
- 파일 연산
	- 기본 연산 : 생성, 쓰기, 읽기, 위치 재설정, 삭제, 절단 등
	- 기타 연산 : 추가, 복사, 이름 변경, 속성 획득/설정
	- open 연산 : 메모리에 있는 open file talbe에 파일에 대한 정보를 등록하고 인덱스 반환
	- close 연산 : open file table에 있는 항목 제거
- open file table
	- open()함수에 의해 open된 파일의 list 형태로 나타낸 테이블. 현재 열린 파일들
	- 2단계 open table - 여러개의 프로세스가 동시에 open
	- 프로세스 단위 테이블(프로세스가 관리)
		- 프로세스 종속 정보 저장, system-wide table에 대한 포인터
	- 전체 시스템 테이블(os가 관리)
		- 프로세스 독립 정보, file open counter가 이 되면 파일 테이블에서 제거

## 10.2 접근 방법(Acess Methods)
---

1. 순차 접근(sequential access)
	- 테이프처럼 앞에서 차례대로 순서대로 접근함
	- 연산 : read next, write next, position to n 등
2. 직접 접근(direct access)
	- 디스크 모델과 같이 직접 접근
	- 임의 순서로 접근 허용(random access)
	- 어느곳이든지 액세스 시간은 똑같음
	- 연산 : read n, write n
3. 인덱스 순차 접근(indexed sequential access)
	- (key, data) 형식의 레코드들을 순서대로 정렬
	- 파일에 대한 index 파일 생성
	- 1차로 index 검색, 2차로 포인터를 사용하여 파일을 직접 접근함
	- 현재는 잘 사용하지 않음
	- DB에서 관리할 때 사용

## 10.3 디렉터리와 디스크 구조
---

- 저장된 파일을 체계적으로 관리하기 위해 디렉터리 필요

![10 3](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/132db82d-7aab-4ead-acae-3e2143488330)

- 디스크를 여러 부분으로 분할하여 파티션(Partition) 생성
- 각 파티션에 file system을 생성
	- file system은 directory 구조를 사용하여 directory들을 생성
- Directory는 디렉토리에 있는 파일에 대한 정보들을 저장

### 디스크 구조

- 디스크는 여러개의 파티션으로 분할 가능
- disk/partition은 파일 시스템이 없는 raw 형태와, 파일 시스템이 있는 formatted 형태로 사용 가능
- 볼륨(volume)
	- 파일 시스템을 포함한 개체(entity)
	- 전체 장치, 장치의 부분집합 또는 RAID로 연결된 다수 장치 일 수 있음
	- 각 볼륨은 논리적 가상 디스크로 취급될 수 있음

### 디렉터리의 논리적 구조

- 디렉터리 구조 구성 시 고려사항
	- 효율 : 파일 위치를 신속하게 검색
	- 명명(naming) :  사용자가 편리하게
	- 그룹핑 : 파일 특성에 따른 논리적 그룹핑

- 디렉터리의 논리적 구조
	- 1단계 디렉터리(single-level directory)
		- ![10 4_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/af795813-34eb-4542-ab9e-8222a4829f45)
		- 모든 사용자가 한개의 디렉터리 사용
		- 단점 : 같은 이름 사용 불가, 그룹핑 불가
	- 2단계 디렉터리(two-level directory)
		- ![10 4_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/e7589621-95eb-4224-b283-0adad9566a69)
		- 각 사용자가 분리된 디렉터리 사용
		- 단점 : 그룹핑 불가
	- 트리 구조 디렉터리(tree-structured directory)
		- ![10 4_3](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/8b915841-cb98-4574-b691-44f8a2eb6b24)
		- 효율적 파일 검색 가능, 그룹핑 가능, 경로를 사용한 이름 가능
		- 현재 디렉터리(작업 디렉터리)
	- 비순환 그래프 디렉터리(acyclic-graph directory)
		- ![10 4_4](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/32a3b684-e5e5-45aa-a051-bb1a222ed28d)
		- 디렉터리간의 파일, 서브 디렉터리 공유 허용
		- 심볼릭 링크(symbolic link) : 다른 파일/디렉터리에 대한 포인터 저장
		- 하드 링크(hard link) : 저장장치가 파일 위치를 직접 저장
		- 같은 파일이 여러개의 경로 이름을 가질 수 있음(aliasing)

## 10.4 File-System Mounting
---

- 파일 시스템 Mounting
	- 보조 기억장치에 설치된 파일 시스템에 접근할 수 있도록,    
	  현재 파일 시스템의 특정 디렉터리에 연결하는 것
- Mounting 절차
```
1. 디바이스 이름과 mount point(빈 디렉터리)를 운영체제에 제공
2. 운영체제는 디바이스가 유효한 파일시스템을 포함하고 있느지 검증함
3. 파일시스템은 저장된 mount point에 마운트됨
```

### 10.5 File 공유
---

- 다수의 사용자의 파일 공유
- 원격 파일 시스템(Remote File System)
	- Cilent-Server 모델을 사용하여 구현 가능
	- NFS
	- CIFS 

## 10.6 보호
---

- 접근 제어
	- 파일 생성자/소유자의 접근 권한 제어
- UNIX에서는 접근 리스트(Access List)를 사용함
	- 접근 모드 : read, write, execute
	- 파일이나 디렉터리에 대한 접근 권한 정의