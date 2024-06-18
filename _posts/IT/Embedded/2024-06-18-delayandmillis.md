---
title: "아두이노 Delay와 millis"
date: 2024-06-18 07:28:00 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [arduino, delay, millis]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://cdn-learn.adafruit.com/guides/cropped_images/000/000/336/medium640/vfdon.jpg?1520540872" 
    alt: 
---

프로젝트를 구현하던 중, 구조에서 문제가 발생했다

매 초 온습도 센서를 통해 데이터를 체크하고, 

5분 간격으로 NTP 서버에 시간 정보를, Openweather에서 날씨 정보를 얻어오는 프로젝트였다

간단한 구조라 `delay()`로 구현하였는데, 시간을 출력하는 부분에서 미세하게 오차가 발생했다

## delay

- delay 함수는 가장 쉽게 사용할 수 있는 명령 지연 함수
- 프로그램 실행을 중단 시키기 때문에, 그 시간동안 다른 작업을 할 수 없음(Blocking)
- 짧은 시간동안의 지연이 필요한 경우 사용
- 동시에 다른 작업을 수행할 수 없으므로, 단일 작업 구현에 사용
- 시간 지연이 정확하지 않을 수 있음
- CPU가 대기하면서 전력을 소비하기 때문에, 전력 소모량이 높음 

```c
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // LED 켜기
  delay(1000);                       // 1초 대기
  digitalWrite(LED_BUILTIN, LOW);    // LED 끄기
  delay(1000);                       // 1초 대기
}
```

## millis

- 아두이노가 실행된 후 경과 시간을 밀리초 단위로 반환하는 함수
- 프로그램의 실행을 중단하지 않음
- 비동기식 타이머로 여러 작업을 병행할 수 있음
- 일정 주기로 특정 작업을 수행하는데 유용함

```c
unsigned long previousMillis = 0; 
const long interval = 1000;  // 1초 간격

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  unsigned long currentMillis = millis(); // 실행 경과 시간을 받아옴

  if (currentMillis - previousMillis >= interval) { // 현재 시간과 이전 시간을 비교하여 간격을 초과했는지 체크
    previousMillis = currentMillis;  // 이전 시간을 현재 시간으로 갱신

    // LED 상태 토글
    digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
  }
}
```

최대한 millis를 이용해 시간 차이를 이용해 구현하고, delay 사용을 지양할 것