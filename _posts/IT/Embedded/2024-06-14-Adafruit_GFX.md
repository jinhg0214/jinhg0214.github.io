---
title: "Adafruit GFX 라이브러리"
date: 2024-06-14 21:27:00 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [tft, screen, library, adafruit, graphic, gui]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://cdn-learn.adafruit.com/guides/cropped_images/000/000/336/medium640/vfdon.jpg?1520540872" 
    alt: 
---

## Adafruit GFX
---

Adafruit 사에서 제공하는 그래픽 라이브러리

다양한 도형 및 텍스트를 출력하는 기능들을 포함하고 있음

소형 마이크로컨트롤러에 사용되며, 다양한 LCD 및 OLED 디스플레이에 그래픽을 출력하는데 사용함

## 장점
---

- 쉽게 사용할 수 있음

- 다양한 디스플레이 모듈과 호환됨. 다양한 TFT 스크린 라이브러리들이 이를 기반으로 작성됨

- 오픈소스 라이브러리

- 아두이노 보브들, ESP8266/ESP32, Raspberry Pi 등에서 사용함

## 간단 설명
---

1. 설치
2. 좌표계 및 단위
3. 그래픽 프리미티브
4. 화면 회전
5. 폰트 및 글꼴

[https://learn.adafruit.com/adafruit-gfx-graphics-library](https://learn.adafruit.com/adafruit-gfx-graphics-library) 공식 메뉴얼을 참조하여 작성함


### 1. 설치
아두이노 IDE에서 라이브러리 매니저로 쉽게 설치 가능

gfx만 검색해도 `adafruit gfx library`라고 뜨는데 이를 설치

## 2. 좌표계 및 단위
---

![image](https://cdn-learn.adafruit.com/assets/assets/000/001/264/medium800/lcds___displays_coordsys.png?1396770439)

- horizontal : X, vertical : Y 좌표계 이용
	- 왼쪽 상단이 (0,0) 우측 하단이 (X+W, Y+H)
- 픽셀 단위로 표현. 밀리미터나 인치 사용하지 않음

![color](https://cdn-learn.adafruit.com/assets/assets/000/001/265/medium800/lcds___displays_colorpack.png?1396770449)
- 색상 지원 디스플레이 경우. 색상은 부호 없는 16비트 값으로 표시
	- http://www.barth-dev.de/online/rgb565-color-picker/ 16bit RGB565 색상에 대한 설명

## 3. 그래픽 프리미티브
---

![graphic](https://cdn-learn.adafruit.com/assets/assets/000/001/267/medium800/lcds___displays_st7735pixel.jpg?1396770465)
- 장치에 공통적으로 동일하게 동작하는 일반적인 그래픽 기능들
	- 각 라이브러리마다 이를 응용하여 작성하므로, 실제 사용은 해당 라이브러리를  참조할 것
### 도형
##### 픽셀 그리기
 `void drawPixel ( uint16_t x, uint16_t y, uint16_t color);`
(X, Y) 좌표에 색상을 이용하여 점 찍기
##### 선 그리기
![line](https://cdn-learn.adafruit.com/assets/assets/000/001/268/medium800/lcds___displays_line.png?1396770476)
![line2](https://cdn-learn.adafruit.com/assets/assets/000/001/269/medium800/lcds___displays_st7735lines.jpg?1396770485)
`void drawLine ( uint16_t x0, uint16_t y0, uint16_t x1, uint16_t y1, uint16_t color) ;`
시작점과 끝점, 색상을 받아 선 그리기

`void drawFastVLine ( uint16_t x0, uint16_t y0, uint16_t 길이, uint16_t color) ;` 
`void drawFastHLine ( uint8_t x0, uint8_t y0, uint8_t 길이, uint16_t color) ;`
수직선이나 수평선은 별도의 함수 존재

##### 직사각형 그리기
![rect](https://cdn-learn.adafruit.com/assets/assets/000/001/270/medium800/lcds___displays_rect.png?1396770497)
![rect2](https://cdn-learn.adafruit.com/assets/assets/000/001/271/medium800/lcds___displays_st7735squares.jpg?1396770504)
`void drawRect(uint16_t x0, uint16_t y0, uint16_t w, uint16_t h, uint16_t color);` 
`void fillRect(uint16_t x0, uint16_t y0, uint16_t w, uint16_t h, uint16_t color);`
왼쪽 상단 모서리 좌표 (X, Y)와 픽셀 단위의 너비와 높이, 색상를 받아 직사각형을 그림

##### 원 그리기
![circle](https://cdn-learn.adafruit.com/assets/assets/000/001/272/medium800/lcds___displays_circle.png?1396770516)
![circle2](https://cdn-learn.adafruit.com/assets/assets/000/001/273/medium800/lcds___displays_st7735circles.jpg?1396770524)
`void drawCircle ( uint16_t x0, uint16_t y0, uint16_t r, uint16_t color) ;` 
`void fillCircle ( uint16_t x0, uint16_t y0, uint16_t r, uint16_t color) ;`
중심점 (X,Y), 반지름, 색상으로 원 그리기 

##### 둥근 직사각형
![rrect](https://cdn-learn.adafruit.com/assets/assets/000/001/274/medium800/lcds___displays_roundrect.png?1396770535)
`void drawRoundRect ( uint16_t x0, uint16_t y0, uint16_t w, uint16_t h, uint16_t 반경, uint16_t color) ;`
`void fillRoundRect ( uint16_t x0, uint16_t y0, uint16_t w, uint16_t h, uint16_t radius, uint16_t color) ;`
왼쪽 상단 좌표 (X, Y)와 너비, 높이, 그리고 모서리 반경과 색상을 받아 그림

##### 삼각형 
![tri](https://cdn-learn.adafruit.com/assets/assets/000/001/275/medium800/lcds___displays_triangle.png?1396770547)
`void drawTriangle ( uint16_t x0, uint16_t y0, uint16_t x1, uint16_t y1, uint16_t x2, uint16_t y2, uint16_t color) ;` 
`void fillTriangle ( uint16_t x0, uint16_t y0, uint16_t x1, uint16_t y1, uint16_t x2, uint16_t y2, uint16_t color) ;`
세 모서리 점과 색상을 받아 삼각형을 그림

### 텍스트
##### 단일 텍스트 
![text](https://cdn-learn.adafruit.com/assets/assets/000/001/276/medium800/lcds___displays_char.png?1396770557)
`void drawChar(uint16_t x, uint16_t y, char c, uint16_t color, uint16_t bg, uint8_t size);`
(X, Y)좌표에 문자 하나를, 색상, 배경색 지정하여 크기 설정 후 출력

##### 문자열

![string](https://cdn-learn.adafruit.com/assets/assets/000/001/277/medium800/lcds___displays_text.jpg?1396770564)
print()를 이용한 출력. 출력 전 세팅이 필요함

```c
void setCursor(int16_t x0, int16_t y0);  // 텍스트 커서
void setTextColor(uint16_t color);  // 텍스트 색상
void setTextColor(uint16_t color, uint16_t backgroundcolor); // 텍스트 색상  + 배경색
void setTextSize(uint8_t size); // 텍스트 크기
void setTextWrap(boolean w); // 줄바꿈
```
이후 print()를 이용해 출력 가능

`setTextSize()`는 1,2,3,4,5 중 하나 선택 가능 1은 10, 2는 20 이런식.


## 4. 회전 및 디스플레이
---
`void setRotation(uint8_t rotation);` 
0, 1, 2, 3으로 회전 가능. 각각 0, 90, 180, 270도씩 회전함
이미 그려진 화면은 회전하지 않으므로, 그리기 전에 회전시켜두어야함

회전 시, 원점인 (0, 0)이 변할 수 있음. 별도로 세팅이 필요함

## 5. 폰트, 글꼴
---
기본 폰트는 GNU FreeFont 프로젝트 사용. 필요 시 폰트 추가 가능
Adafruit_GFX 내부의 "Fonts" 폴더 내에 사용 폰트들 확인 가능

```
FreeMono12pt7b.h FreeSansBoldOblique12pt7b.h 
FreeMono18pt7b.h FreeSansBoldOblique18pt7b.h 
FreeMono24pt7b.h FreeSansBoldOblique24pt7b.h 
FreeMono9pt7b.h FreeSansBoldOblique9pt7b.h
```
각 파일 이름은 서체 이름()"FreeMono", "FreeSerif" 등)로 시작하고, 스타일, 글꼴 크기 및 7b(ASCII 코드 )가 포함되어있음을 나타냄
8비트 글꼴은 아직 제공되지 않음

Adafruit_GFX및 디스플레이별 라이브러리를 포함한 후, 폰트 파일를 포함하여 사용 가능

```c
# include <Adafruit_GFX.h> // 핵심 그래픽 라이브러리 
# include <Adafruit_TFTLCD.h> // 하드웨어 관련 라이브러리 
# include <Fonts/FreeMonoBoldOblique12pt7b.h>  // 다음과 같이 폰트 추가
# include <Fonts/FreeSerif9pt7b.h>
```
글꼴 파일은 꽤 많은 공간을 차지하므로, 주의할 것
표준 내장 글꼴을 사용하면 이를 줄일 수 있음
`tft.setFont(&FreeMonoBoldOblique12pt7b);` 로 글꼴을 세팅할 수 있음
`tft.setFont();` 로 NULL값을 매개변수로 사용시 초기 폰트로 변경 가능

[MagTag Quotes 예제](https://learn.adafruit.com/adafruit-magtag/quotes-example) 에서 사용자 정의 글꼴 전체 예제 확인 가능

### 새 글꼴 추가
새로운 폰트를 추가하기 위해서는 gcc 컴파일러와 FreeType 라이브러리가 필요함
Fontconvert를 사용하여 makefile로 make 해야함
`./fontconvert myfont.ttf 12 > myfont12pt7b.h`
