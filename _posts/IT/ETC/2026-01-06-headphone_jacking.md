---
title: "Airoha 칩셋의 치명적 결함: 페어링 없이 내 폰을 조작하는 'Headphone Jacking'"
date: 2026-01-06 11:30:00 +0900
categories: [IT, security]  # 최대 2개 가능
tags: [bluetooth, hacking, vulnerability]     
toc: true
comment: false
published: true
image:
    path: "https://pbs.twimg.com/media/G9NSOebXIAMnjB0.jpg"
    alt: 
---

우리가 매일 쓰는 무선 이어폰이 사실은 스마트폰의 백도어가 될 수 있다면?

<iframe width="560" height="315" src="https://www.youtube.com/embed/olFO55cLvuE?si=fQ2aycjrFN9fu36E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

HITCON에서 Enno Rey Netzwerke (ERNW)의 보안 연구원 

Dennis Heinze, Julian Suleder, Frieder Steinmetz가 발표한 

'Headphone Jacking : A Key to Your Phone'의 정리 내용

---
## 세줄 요약
1. 대만 Airoha 무선 이어폰 칩셋의 관리자용 프로토콜(RACE)의 보안 결함으로, 공격자가 페어링 없이 이어폰 메모리에 무단 접근할 수 있음
2. 공격자는 메모리에서 암호화 키(Link Key)를 탈취한 뒤, 진짜 이어폰인 척 위장하여 스마트폰과 신뢰 관계를 형성함
3. 이를 통해 해커는 잠긴 스마트폰에서 음성 비서를 조작하거나, 전화를 걸거나, 메세지를 보내는 등 기기의 제어권을 일부 장악할 수 있었음 

---

## 헤드폰 재킹(Headphone Jacking)이란?

- 과거에는 헤드폰 잭을 이용한 물리적 연결을 사용했다면, 이제는 대부분의 사용자들이 블루투스 무선 이어폰을 사용함

- 무선 연결(Pairing) 과정에서 이어폰 내부 칩셋의 취약점을 이용하여 공격하는 방법을   
Headphone Jacking으로 정의하며, 이것이 스마트폰으로 들어가는 마스터키가 될 수 있음을 보임

- 무려 페어링 없이 메모리에 접근 가능하다고 함

---

## 취약점 발견 과정

![headphone](https://i.ytimg.com/vi/y5vnSQBocyM/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLAuqQg0Kgsd_z8KDPFLPPl1BJGh3g)

- 저가형 헤드폰으로 테스트
	- 블루투스 오라캐스트(Auracast)를 이용해서 헤드폰의 블루투스 신호를 Wireshark로 확인해봄
	- 패킷을 분석해본 결과 펌웨어 업데이트 처리 프로토콜등이 발견됨
	- 메모리 특정 부분에 쓰기 작업을 수행하는 명령어
	- 플래시 메모리를 읽을 수 있는 명령어 또한 존재
	- 이를 이용해 USB HID를 통해 플래시 메모리를 덤프할 수 있었다고 함

![earbuds](https://cdn.thewirecutter.com/wp-content/media/2025/10/BEST-WIRELESS-BLUETOOTH-EARBUDS-2048px-5969B-2x1-1.jpg?width=2048&quality=75&crop=2:1&auto=webp)

- 이를 저가형 헤드폰이 아니라, AIROHA transmitter를 사용하는 유명 무선 이어폰에 적용해봄
	- 구글의 파이썬 기반 블루투스 라이브러리 'Bumble' 을 이용하여 툴을 개발함
	- **RACE(Remote Access Control Engine)** 라는 이름의 Airoha 전용 프로토콜을 발견
	- 해당 프로토콜은 제조사가 기기 점검이나 펌웨어 업데이트를 위해 마련해둔 관리자 전용 통로
	- 기기 내부의 플래시 메모리와 RAM의 특정 주소를 읽고 쓸수 있는 명령어가 포함됨
	- **그러나 이 과정에서 페이링은 필요하지 않았다**
	- 블루투스 페어링을 하지 않은 상태에서도 이 명령어로 신호 범위 내에서 헤드폰의 메모리에 바로 접근할 수 있었다고 함
- 클래식 블루투스는 'ubertooth one'과 같은 스니핑 장치를 이용하여 블루투스 장치와 해당 주소를 찾을 수 있음
- 특히 많이 사용하는 Bluetooth Low Energy, BLE에서는 GATT 프로토콜을 사용하기 때문에 위 장치가 필요 없음
	- 마찬가지로 페어링 할 필요 없었음. BLE 기기들이 자신을 advertisements하기 때문에 구독만 해서 RACE 프로토콜을 이용해 메모리를 읽고 쓸 수 있었다고 함

---

## 해킹 취약점
- 스마트폰 자체의 결함이 아니라, 스마트폰이 신뢰하는 주변 기기를 공략하여 그 신뢰를 가로채는 점
- 코드 실행, 도청, 데이터 추출
- 페이링 키 복사
	- 플래시 메모리가 있는 모든 블루투스에 적용되는 사항
	- 스마트폰의 주소, 헤드폰의 주소, 그리고 이 링크 키를 알고 있다면, 헤드폰을 사칭할 수 있음
	- 이를 이용하여 핸즈프리 프로파일(HFP)의 모든 권한을 습득할 수 있음
		- 전화, 오디오 라우팅, 자신의 번호 노출, 네트워크 정보, 보이스 어시스턴트를 이용한 모든 기능(문자 발송등)

---

## 대응 

현재 Airoha의 칩을 사용하는 벤더들이 너무 많아 복잡하게 얽혀있어서, 

어떤 기기가 이 해킹에 노출되어있는지 파악이 불가능한 상황이라고 함

Airoha에서 SDK 업데이트를 25년 6~8월쯤 출시했고, 

벤더들이 업데이트들을 했지만 해당 보안 취약점에 대해서는 해결하지 못했다고 함

![Image](assets\img\posts\20260106\1.JPG)

연구진들이 확인한 취약한 블루투스 기기들(여기에 없어도 공격에 노출되어 있을수도 있음)

**사용자 조치**

- 펌웨어 업데이트
- 불필요한 페어링 기기들 삭제
- 잠금 화면 음성 비서 제한
- 자신이 취약하다고 생각하면 유선 기기 사용하기

스마트폰은 점점 더 많은 권한을 가지고, 

주변 기기들은 공격자들에게 더욱 흥미로운 타겟이 되었다.