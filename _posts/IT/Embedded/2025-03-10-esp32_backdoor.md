---
title: "ESP32 문서화되지 않은 명령어 논란"
date: 2025-03-10 13:57:00 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [esp32, security, backdoor, bluetooth]   
toc: true
comment: false
published: true
image:
    path: "https://www.bleepstatic.com/content/hl-images/2025/03/07/esp32.jpg" 
    alt: 
---

## 🚨ESP32 백도어 논란, 사실일까?
---

![esp32](https://www.bleepstatic.com/content/hl-images/2025/03/07/esp32.jpg)

25년 3월 8일, ESP32 칩에 백도어가 존재한다는 연구 결과가 발표되었다

ESP32는 중국 제조업체 **Espressif**에서 만든 **WiFi & Bluetooth 지원 IoT 칩**으로, 

23년 기준 10억 개 이상의 장치에 널리 사용되는 마이크로칩이다

만약 이 보안 취약점이 사실이라면, 누군가가 무선으로 장치를 해킹하고 네트워크를 장악할 수 있으므로

**전 세계 컴퓨팅 환경에 심각한 보안 위협**이 발생할 수 있다

## 🔍스페인의 보안 연구팀, "ESP32 백도어 발견" 주장

![rootedCON](https://hackercar.com/wp-content/uploads/2019/03/rootedcon.jpg)

스페인 마드리드에서 열린 컴퓨터 보안 컨퍼런스인 RootedCON에서 

스페인의 정보 보안 기업인 Tarlogic은 

ESP32칩에서 Bluetooth 관련 비공개 명령어 29개를 발견했다고 발표했다

연구팀은 Bluetooth 기능에 대한 저수준 제어를 허용하는 ESP32 Bluetooth 펌웨어에서 

숨겨진 제조업체 전용 명령(Opcode Ox3F)를 발견하고, **이를 ESP32 백도어**라고 주장했다

![Image](https://github.com/user-attachments/assets/8e480152-8fd4-4d30-893c-2309c67403b3)

이들은 총 29개의 문서화 되지 않은 명령을 발견했는데, 이를 '백도어' 가능성이 있는 기능으로 설명했다

이는 메모리 조작(RAM 및 플래시 읽기/쓰기), MAC 주소 스푸핑 및 LMP/LLCP 패킷 주입에 사용될 수 있었다

![memorymap](https://www.bleepstatic.com/images/news/u/1220909/2025/March/diagram.jpg)

## ❌ 진짜 백도어? 사실은 단순한 미공개 기능일 뿐
---

그러나 실제로 이 명령어들은 HCI(Host to Controller Interface)에서만 실행할 수 있으므로,

이는 기존에 호스트가 이미 해당 장치를 제어하고 있어야 한다는 의미이다

HCI(Host to Controller Interface)란 Bluetooth 통신을 담당하는 인터페이스로, 

컴퓨터와 Bluetooth 칩(컨트롤러) 간의 명령을 주고 받는 역할을 한다

즉 ESP32 내부의 Bluetooth 기능을 제어하려면, 호스트(컴퓨터)에서 명령을 내려야 한다

다시 말해, 해커가 이 명령어를 사용하려면 다음과 같은 조건을 만족시켜야한다

1. ESP32 칩이 연결된 호스트에 이미 코드 실행 권한을 얻은 상태

2. 이후 호스트에서 ESP32 칩으로 명령을 보내야 함

즉, 공격자가 이 명령어를 실행하려면 이미 호스트를 장악한 상태여야 한다

따라서 이를 '백도어' 라고 부르는 것은 과장이며, 단순히 미공개된 기능에 가깝다고 정리할 수 있다


## 참조
---
[# Undocumented commands found in Bluetooth chip used by a billion devices](https://www.bleepingcomputer.com/news/security/undocumented-commands-found-in-bluetooth-chip-used-by-a-billion-devices/)

[# did they really find a backdoor in 1 billion devices? (esp32 drama)](https://youtu.be/ndM369oJ0tk?si=VJT22qhm1R6KjXfx)
