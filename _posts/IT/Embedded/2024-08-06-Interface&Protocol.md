---
title: "임베디드 시스템 - 인터페이스 및 프로토콜"
date: 2024-08-06 09:58:23 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [roadmap, interface, protocol]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://mlfk3cv5yvnx.i.optimole.com/cb:k-rC.2fd1d/w:auto/h:auto/q:mauto/f:best/https://www.ninjaone.com/wp-content/uploads/2024/02/12-types-of-network-protocols-a-comprehensive-guide.jpg" 
    alt: 
---

임베디드 시스템 엔지니어링 로드맵 중

인터페이스 및 프로토콜에 관한 부분

## 1. 인터페이스와 프로토콜

![interface](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQtEB4g3bOJIbpirmLtrOWAgXr_CjRRlUbkUA&s)

> 인터페이스(Interface)
>
> 서로 다른 두개의 시스템, 장치 사이에서 정보나 신호를 주고받는 경우의 접점이나 경계면
>
> 컴퓨팅에서 컴퓨터 시스템끼리 정보를 교환하는 공유 경계

즉, 인터페이스는 두 장치가 서로 대화를 나누는 방법을 의미한다

예를들어, 두 사람이 대화를 나눌때는 직접 목소리로, 떨어져 있을때는 전화를 쓴다

이처럼 두 장치가 데이터를 주고 받을 때 사용하는 특별한 방법이나 도구가 인터페이스이다

![protocol](https://miro.medium.com/v2/resize:fit:1004/0*MDhXoP_Ka4jo0k7M.png)

> 프로토콜(Protocol)
>
> 통신 프로토콜, 통신 규약은 컴퓨터나 원거리 통신 장비에서 메시지를 주고 받는 양식과 규칙의 체계
>
> 신호 체계, 인증, 그리고 오류 감지 및 수정 기능을 포함할 수 있다

한번 두드리면 1번, 두 번 두드리면 2번인 것 처럼

어떤 형식으로 주고 받을지 정해둔 약속

규칙을 정해두어 서로 이해하고 정확하게 대화 할 수 있도록 함

## 2. 임베디드 시스템에서 통신 프로토콜

![embedded](https://media.licdn.com/dms/image/D4D12AQGCiSSJlSGbiA/article-cover_image-shrink_720_1280/0/1678260515650?e=2147483647&v=beta&t=vEcv0IEYqjZJijiM-1c1IClU99cznqu9BuGFZ8939_g)

임베디드 시스템의 통신은, 

특정한 환경과 요구 사항에 맞게 설계되기 때문에 몇가지 특징이 있음

### 1. 제한된 기능
- 특정 기능이나 작업을 수행하도록 설계되어있어, 일반적인 범용 컴퓨터보다 기능이 제한적임
- 전자레인지는 조리 시간과 온도 조절만 담당하지, 멀티태스킹이나 데이터 처리는 없음

### 2. 제약된 크기
- 하드웨어와 소프트웨어가 제한된 공간 내에서 작동해야하므로, 부품의 크기에 제한이 있음
- 스마트폰같이 제한된 사이즈 내에서 통신을 수행해야함

### 3. 저전력
- 배터리로 구동되거나, 전력 사용이 중요한 환경에서 사용되므로, 전력 소모를 최소화 해야함
- 스마트워치는 배터리 수명을 늘리기 위해 전력 사용을 최소화하는 모드와 프로토콜이 존재

### 4. 실시간성
- 실시간으로 데이터를 처리하고 응답해야하는 경우가 많음
- 전기 자동차의 기능들은 실시간 피드백이 필요함

### 5. No HDD -> ROM, RAM, Flash Memory
- 하드 디스크 드라이브와 같은 대용량 저장장치를 사용하지 않음

이런 특성을 갖기 때문에 임베디드 시스템의 통신은 

시스템 설계 시 중요한 고려사항이며, 전체적인 성능과 신뢰성에 큰 영향을 미친다


## 3. 통신의 종류

![image](https://github.com/user-attachments/assets/6a7c5caa-a768-4836-9dea-d00eabf0ebb5)

간단하게 어떤 종류가 있는지만 언급하고, 각각 자세히 다루어볼 예정

### Basic 
1. UART (Universal Asynchronous Receiver/Transmitter)
    - 두 장치간의 직렬 통신을 위해 사용되는 비동기 통신 방식
2. I2C (Inter-Integrated Circuit)
    - 두개의 선으로 다중 장치를 연결할 수 있는 동기식 직렬 통신 프로토콜
3. SPI (Serial Peripheral Interface)
    - 동기식 직렬 통신 프로토콜, 마스터 슬레이브 구조를 가짐

### High-Speed
1. Ethernet
    - 컴퓨터 네트워크에서 데이터 전송을 위한 표준 기술
2. USB(Universal Serial Bus)
    - 고속 데이터 전송을 위한 범용 직렬 버스
3. PCIe
    - 컴퓨터 포르세서와 확장 장치간의 데이터를 전송하는 고속 직렬 컴퓨터 확장 버스 표준

### Wireless
1. Bluetooth  
    - 개인 근거리 무선 통신 표준
2. Wi-Fi
    - 근거리 무선 망. 전자기기들이 무선랜에 연결할 수 있게 하는 기술
3. LoRa
    - 저전력과 장거리 무선 통신을 위해 설계된 무선 통신 기술
4. Zigbee
    - 저전력, 저비용 무선 메쉬 네트워크 프로토콜
5. Thread
    - IP 기반의 저전력 무선 메쉬 네트워크 프로토콜
6. Matter
    - 스마트 홈 장치간의 상호 운용성을 높이기 위해 Wi-FI, 이더넷, Thread와 같은 기본 기술의 장점을 결합한 IoT 프로토콜
7. UWB
    - 초광대역, 고주파수에서 전파를 통해 작동하는 단거리 무선 통신 프로토콜

### Industrial
1. Modbus
    - 산업용 전자 기기 간 통신을 위한 프로토콜
2. Profinet
    - 자동화 기술을 위한 표준 통신 프로토콜
3. EtherCAT
    - 산업용 이더넷 기술. 실시간 성능 제공
4. MQTT
    - M2M, IoT를 위한 프로토콜로 최소한의 전력과 패킷량으로 통신하느 ㄴ프로토콜
5. CoAP
    - 제약이 있는 장치들을 위한 특수한 인터넷 애플리케이션 프로토콜

### Network
1. TCP/IP
    - 컴퓨터 간 통신 표준 및 네트워크 라우팅 및 상호 연결에 대한 규칙을 지정하는 프로토콜
2. UDP (User Datagram Protocl)
    - 조회 시간에 민감한 정송에 사용하는 프로토콜

### Automotive
1. CAN(Controller Area Network)
    - 자동차 내 각종 전자 제어 장치 간 통신을 위한 프로토콜
2. LIN(Local Interconnect Network)
    - 자동차 내부의 저속 통신을 위한 보조 네트워크 프로토콜
3. MOST
    - 광케이블을 이용하여 제어 정보를 전송하는 시스템
4. FlexRay
    - 고속 데이터 전송이 필요한 자동차 애플리케이션을 위한 통신 프로토콜

### Celluar
1. GSM / LTE
    - 모바일 장치 및 IoT 장치를 위한 고속 무선 통신 표준
2. LTE-M / 5G
    - 차세대 이동 통신 기술, 매우 높은 데이터 전송 속도와 저지연 제공
3. NB-IoT(Narrowband IoT)
    - 저전력 광역 통신을 위한 셀룰러 표준