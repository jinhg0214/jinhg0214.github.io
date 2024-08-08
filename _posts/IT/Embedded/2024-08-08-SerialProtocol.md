---
title: "직렬 통신"
date: 2024-08-08 10:52:13 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [interface, protocol, serial, communication, protocol]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://circuitdigest.com/sites/default/files/projectimage_tut/Serial-Communication-Protocol.png" 
    alt: 
---
[임베디드 시스템 엔지니어링 로드맵](https://jinhg0214.github.io/posts/Roadmap/)

인터페이스 & 프로토콜 파트

## 1. 직렬 통신 (Serial Communication Protocols)
---
![serial](https://cdn11.bigcommerce.com/s-ybeckn7x79/images/stencil/original/image-manager/serial-communication-diagram3.jpg)
- 데이터를 **한 번에 하나의 비트 단위로 순차적으로 데이터를 전송**하는 방식
- 데이터가 **한 줄의 전송 라인**을 통해 순차적으로 전송됨

## 2. 직렬 통신의 특징
---

### 장점
- 적은 선 사용 : 병렬 통신에 비해 전송에 필요한 선의 개수가 적음
- 장거리 전송 : 전송 선이 적어, 신호 간섭이 줄어들어 상대적으로 장거리 전송에 유리함
- 간단한 연결 : 병렬 통신보다 연결이 간단하고, 하드웨어 비용이 절감됨

### 단점
- 속도 제한 : 한 번에 한 비트씩 전송하므로, 전송 속도가 병렬 통신에 비해 느림
- 전송 속도 의존성 : 데이터 전송 속도가 높아질수록, 신호의 왜곡이나 간섭 가능성이 증가
- 동기화의 어려움 : 송신자와 수신자의 동기화가 어려울 수 있음

## 3. 직렬 통신의 구조
---
- 공통적인 기본 구조
- **데이터 라인**: 데이터를 전송하는 선(Tx)과 데이터를 수신하는 선(Rx)이 기본적인 구조를 이룸
	- **송신기 (Transmitter)**: 데이터를 직렬로 변환하여 전송
	- **수신기 (Receiver)**: 직렬로 수신된 데이터를 다시 병렬로 변환하여 처리
- **클럭 라인 (동기식 통신에서 사용)**: 동기식 직렬 통신에서는 송신자와 수신자가 동일한 클럭 신호를 사용함
- **제어 신호 라인 (선택 사항)**: 흐름 제어나 오류 검출을 위한 추가적인 신호 선을 포함하기도 함

## 4. 직렬 통신 구조의 예
---
- UART
- 모스 부호, 전보
- RS-232
- RS-422
- RS-485
- I²C
- ARINC 818
- 범용 직렬 버스(USB)
- IEEE 1394
- 이더넷
- 파이버 채널
- 인피니밴드
- MIDI
- DMX512
- SDI-12
- 직렬 결합 SCSI
- SATA
- 스페이스와이어
- PCI 익스프레스
- SONET, SDH
- T-1, E-1
- 모드버스
- USART