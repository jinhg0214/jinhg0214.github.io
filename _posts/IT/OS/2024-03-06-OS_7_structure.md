---
title: "운영체제 2장 - 프로그램, 설계와 구현, 구조"
date: 2024-03-07 15:00:00 +0900
categories: [Computer, OS]  # 최대 2개 가능
tags: [program, mechanism, policy, structure, unix, mac, windows, msdos, kernel]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://miro.medium.com/v2/resize:fit:636/1*LEp6Tu9LKTF0m0DXvgNMvg.png"
    alt: 
---

# 2.5 시스템 프로그램
---

- 시스템 프로그램
	- 프로그램 개발과 실행을 위해 편리한 환경을 제공하는 프로그램
	- 파일관리, 상태정보, 파일 변경, 프로그래밍 언어 지원, 프로그램 적재 및 실행, 통신, 서비스, deamon, 서브 시스템 등
- 응용 프로그램
	- 일반적인 사용자들이 사용하는 프로그램들
- 운영체제에 대한 대부분의 사용자 관점은 system call이라기 보다는 시스테 ㅁ프로그램과 응용 프로그램에 의해 정의됨

# 2.6 운영체제의 설계와 구현
---

- 하드웨어와 시스템 유형에 따라 다름
- 기법(Mechanism)과 정책(Policy)을 분리하여 설계함
	- 기법은 cpu 보호를 위해 타이밍 구조를 사용하는 것 처럼 바뀌지 않는 것
	- 정책은 특장 사용자를 위해 타이머 양을 결정하는 것처럼 바뀔 수 있는것
	- 유연성을 위해 분리가 중요함
### 시스템 구현
- 초기 운영체제는 어셈블리 언어루 작성됨
- 현재 대부분의 운영체제는 고급 언어로 작성됨
	- 커널 수준의 코드는 어셈블리 언어로 작성됨
- 고급 언어 구현의 장단점 
	- 장점 : 코드를 빠르게 작성, 이해와 디버깅이 쉽고, 이식하기 쉽다
	- 단점 : 속도가 느려짐, 소요 메모리 증가 주장
		- 그러나 현대 최적화 컴파일러는 훨씬 우수한 성능
		- 현재 OS의 주된 성능 향상은 자료구조와 알고리즘에 의한것이므로 차이가 없다

# 2.7 운영체제 구조
---

- 단층적 구조 Simple structure : 많은 기능들이 하나의 계층으로 구현됨
- 계층적 구조 Layered structure : 운영체제가 여러 계층으로 구분됨
- Microkernel : 필수적이 아닌 구성요소를 커널에서 모두 제거하고, 시스템 및 사용자 수준 프로그램으로 구현함

### 단층적 구조

![2 7_1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/220b6179-5eb4-4e9a-84f4-3e4eab8a3cea)
- 장점 : 커널 내부 통신 오버헤드가 거의 없어 성능이 뛰어남
- 단점 : 구현과 유지보수가 어려움
- MS-DOS : 최소 공간에 최대 기능을 제공하도록 작성
- UNIX : 초기 하드웨어에 기능 제한이 있었으며, 제한된 구조를 가짐
	- kernel과 system program으로 구성
	- kernel은 여러 인터페이스와 장치 드라이버로 분리되어 확장됨

### 계층적 구조

![2 7_2](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/adcf8725-91d5-46a0-bf24-fc032c5af144)
- 장점: 하위 계층에 어떻게 구현되었는 지 알 필요 없음. 무슨 일만 하는지 알면 됨
- 단점 : 각 계층을 적절히 정의하기 어려움. 비효율적(통신 오버헤드 발생)
- 최하위 계층(layer 0) : 하드웨어
- 최상위 계층(layer N) : 사용자 인터페이스
- Modularity
	- 각 계층은 하위 계층에서 제공하는 함수와 서비스만 사용함
### Microkernel

![2 7_3](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/d28d054c-a151-4ef2-966b-fb7211630638)
- UNIX 커널이 확장되면서 관리하기 어려워져서, 이를 해결하기 위해 Mach 커널 개발
- 장점 : 확장성, 운영체제 이식성, 높은 신뢰성과 보안성
	- 커널에서 수행되는 코드가 적고, 사용자 프로세스로 실행되기 때문
- 단점 : 시스템 함수의 오버헤드 발생. 통신 오버헤드 발생
- 커널의 필수적이 아닌 많은 부분을 사용자 공간으로 이동
- 마이크로커널의 기본 기능
	- 사용자 모듈과 사용자 공간에서 수행하는 서비스간의 통신 기능 제공(Message passing)

### Modules

![2 7_4](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/28c63ce4-4ac1-4057-8ee0-104242a07682)
- 코어 컴포넌트와 적재 가능한 코어 모듈로 구성
- 동적으로 적재되어 커널 기능을 확장함
- 모듈 인터페이스의 특징
	- 각 컴포넌트가 분리됨.
	- 계층적 구조와 유사하지만 더 유연함.
	- 마이크로 커널과 유사하지만 더 효율적(message passing을 사용하지 않음)
![2 7_5](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ebdd7de5-b727-4dca-8aaa-ddede864ff40)
- 리눅스의 운영체제 구조
![2 7_6](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ccedcb62-bedf-4da6-bd33-3de3b5c655d1)
- 윈도우의 운영체제 구조
![2 7_7](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/fcfc7d1a-e6d0-47f5-97fc-9aac23a7dd43)
- Mac OS X 운영체제 구조
![2 7_8](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/c66f9031-4460-4869-91fd-5545fa1b62be)
 - IOS과 Android의 운영체제 구조