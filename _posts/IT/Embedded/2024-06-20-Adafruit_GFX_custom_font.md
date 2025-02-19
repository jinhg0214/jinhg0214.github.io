---
title: "Adafruit GFX 커스텀 폰트"
date: 2024-06-20 16:17:00 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [adafruit, gfx, custom, font, symbol]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://cdn-learn.adafruit.com/guides/cropped_images/000/002/622/medium640/g000-027.jpg?1563121635" 
    alt: 
---

Adafruit 사에서 제공한 Creating Custom Symbol Fonts for Adafruit GFX Library를 정리한 내용

https://cdn-learn.adafruit.com/downloads/pdf/creating-custom-symbol-font-for-adafruit-gfx-library.pdf

위 링크에서 다운로드 가능

간단히 설명하면

원하는 폰트를 컴파일하는 과정이 아닌, 

미리 커스텀 된 폰트에 16x16 픽셀 그림을 하나씩 만들어 넣는 방법임

## 개요
--- 
- Adafruit GFX 라이브러리는 9-24 크기의 폰트 제공
- mono space font와 유사한 Courier, Arial, Helvetica 와 유사한 Sans Serif, Times와 유사한 Serif 폰트 기본 제공
	- 각 폰트는 Bold, 기울임 버전 제공
- 32-126 출력 가능한 문자만 제공함. 심볼 타입 폰트에서 볼 수 있는 화살표나 기호는 제공되지 않음
- 글꼴 변환 도구를 이용하여 직접 제작할 수 있음. 그러나 리눅스에서 사용하는 커맨드 라인 유틸리티임
	- PC나 MAC에서는 어려울 수 있음
	- 원하는 심볼을 가진 폰트를 찾아 변환할 수 도 있지만, 직접 만들 수도 있음
- [Adafruit 매뉴얼](https://learn.adafruit.com/adafruit-gfx-graphics-library/using-fonts)에서 Adafruit GFX 라이브러리 사용법과 커스텀 폰트를 사용하는법을 어느정도 학습할 것을 권장함
- 또한, 16진수 표기법과, 비트들을 16진수로 변환하는 것에 대해 어느정도 지식이 필요함 

## Hardware and software requirements
---
- Adafruit GFX 라이브러리를 실행할 수 있는 하드웨어와 이를 출력할 수 있는 TFT 스크린 필요
- 해당 메뉴얼에서는 2.4인치 320x240 FeatherWing TFT를 사용함
	- 해당 보드는 Feather M0 Express(256kb)를 사용함
	- 이 외에도 3.5 inch 480x320 FeatherWing, Adafruit HalloWing and PyGamer displays. 
	- TFT FeatherWings 사용 시, 별도의 Feather board 필요
	- M0 or M4 or any other 32-bit Feather 권장
	- 커스텀 폰트는 메모리를 많이 사용하므로, 32u4와 같은 메모리 에서는 동작 불가!

![esp32](https://m.media-amazon.com/images/I/61JiwY9v5FL._AC_UF894,1000_QL80_.jpg)

![tft](https://www.electronics-lab.com/wp-content/uploads/2018/03/screen.jpg)

필자는 ESP-WROOM-32와 TFT 1.44 스크린을 사용하였음

## Understanding the Font Specification
---
[Creating Custom Symbol Fonts for Adafruit GFX Library](https://github.com/cyborg5/font_test) 에서 예시 파일을 다운로드 후, 아두이노 IDE에서 해당 파일을 연다

코드를 보면

`#include "SymbolMono18pt7b.h"` 를 통해 커스텀 폰트를 포함하고 있다

받은 깃허브의 프로젝트 내의 `\font_test-master\font_test\SymbolMono18pt7b.h` 을 열어보면

bitmaps를 보면 다음과 같이 정의되어있음

```c
const uint8_t SymbolMono18pt7bBitmaps[] PROGMEM = { 
	// 각 픽셀별로 그림이 그려진 모습
};

const GFXglyph SymbolMono18pt7bGlyphs[] PROGMEM = {
  //Index,  W, H,xAdv,dX, dY
  {     0, 16,21, 21, 3,-19}, // 00 test square
  {    42, 16,15, 21, 3,-18}, // 01 Upper_Left_Arrow
  {    72, 16,15, 21, 3,-18}, // 02 Upper_Right_Arrow
  {   102, 16,15, 21, 3,-16}, // 03 lower left arrow
	// ETC..
	{42, 16, 15, 21, 3, -18} // 144
}; 
```

`GFXglyph` 라는 struct의 정의의 배열임을 볼 수 있다

각 구조체의 속성을 살펴보면 

- Index : index into the bitmap array. 
	- 첫번째 glyph는 0에서 시작. `test_square`라는 예제가 42byte이므로, 다음 예제는 42부터 시작하는 것을 볼 수 있음
	- 이런식으로 첫번째 인덱스는, 이전 Index 숫자에 이전 glyph의 길이만큼 더해주면 됨
- W, H : glyph의 width와 height
	- 여기선 16x16을 사용했음.
	- height는 원하는 만큼 설정 가능함.
- xAdv : xAdvance 값. 
	- print, println 함수 사용시, 각 문자 사이에 수평 간격으로 얼마나 간격이 필요한지 알려줌
	- fixed space font이므로, 픽셀 크기로 고정된 21값을 사용함.
		- 가독성을 위해 왼쪽에 2pixel, 오른쪽에 3pixel만큼 여분을 둠
		- glyph의 크기는 16이므로, 16+2+3 총 21픽셀
	- print, println을 사용하는 방식 대신 `drawChar`라는 별도의 함수를 이용해 각 문자를 출력할 것임
- dX, dY : glyph 내에서 어디에 배치할 것인지 결정하는 좌표. 
	- dX는 2-4, dY는 문자의 기준선(최하단)에서 상단까지의 거리 
	- `test_squeare`는 기준선에서 19픽셀 위에서 시작
	- `space`예시는 기준선보다 2픽셀 위에 있음
	- glyph를 수평, 수직으로 조절하기 위해서는 이 값을 사용함


```c
const GFXfont SymbolMono18pt7b PROGMEM = {
  (uint8_t  *)SymbolMono18pt7bBitmaps,
  (GFXglyph *)SymbolMono18pt7bGlyphs,
  0,48, 35 //ASCII start, ASCII stop,y Advance
};
```

- 글꼴 정의의 마지막 부분은 모든것을 하나로 묶는 구조
	- bitmap array의 포인터, glyph데이터에 대한 포인터 및 기타 몇가지 값을 포함함
	- 첫 번째는 첫 번째 문자의 ASCII 값, 
- 파일의 나머지 부분은 사용자 정의 기호 글꼴에 필요한 몇 가지 정의
	- 커스텀 심볼 폰트를 제작하는데 필요한 몇가지 정의들
	- DELTA_C, DELTA_R과 BASE_R값들은 고정된 그리드를 표시하는데 사용
	- 다른 정의들은 정의한 기호들을 참조하는데 사용함

## Using the font display sketch
---
- `font_test` 예제는 지정된 배율 값에서 지정된 문자로 시작하는 글꼴의 일부를 출력함 
- 예제 실행 시
	- Serial 입력을 이용해 이동 가능
	- 0~127을 이용해 해당 폰트를 좌상단에 띄울 수 있음. 
	- -1~-4를 입력해 확대 가능

![KakaoTalk_20240620_161605927_04](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/1935bfcc-9d08-4cbf-9e7e-e88794c47928)

코드를 ST7735 스크린에 맞게 수정하여 실행시킨 모습

해당 코드와 폰트 파일은 [여기서](https://github.com/jinhg0214/adafruit_gfx_custom_font/tree/master/ST7735) 확인 가능

## Creating new glyphs
---

새로운 심볼을 추가해서 만드는 방법

`SymbolMono18pt7b.h` 파일을 열어서 557 라인의 `>>` 기호 찾기

```c
//132 Double greater than
/*| 8 4 2 1 8 4 2 1 8 4 2 1 8 4 2 1 |*/
/*| X . . . . . . , X . . . . . . . |*/  0x80,0x80,
/*| . X . . . . . , . X . . . . . . |*/  0x40,0x40,
/*| . . X . . . . , . . X . . . . . |*/  0x20,0x20,
/*| . . . X . . . , . . . X . . . . |*/  0x10,0x10,
/*| . . . . X . . , . . . . X . . . |*/  0x08,0x08,
/*| . . . . . X . , . . . . . X . . |*/  0x04,0x04,
/*| . . . . X . . , . . . . X . . . |*/  0x08,0x08,
/*| . . . X . . . , . . . X . . . . |*/  0x10,0x10,
/*| . . X . . . . , . . X . . . . . |*/  0x20,0x20,
/*| . X . . . . . , . X . . . . . . |*/  0x40,0x40,
/*| X . . . . . . , X . . . . . . . |*/  0x80,0x80,
```
- 각 라인 주석의 'X' 는 픽셀이 찍히는 곳 `.` 은 빈칸을 의미함
- 행별로 값을 보면 각 값의 16진수값을 확인할 수 있음
	- X, . 값이 중요한 게 아니라 결국 0x00, 0x00, 값이 폰트를 결정함

575라인을 보면 직접 작성 가능한 빈칸을 확인할 수 있음

```c
//   Empty array for future expansion
/*| 8 4 2 1 8 4 2 1 8 4 2 1 8 4 2 1 |*/
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . , . . . . . . . . |*/  0x00,0x00,
```
- 이 내용을 4개 복사하여 아래에 추가하기

- 첫번째 복사본을 "135 diamond suit"로 만들어보기

```c
//135 diamond suit
/*| 8 4 2 1 8 4 2 1 8 4 2 1 8 4 2 1 |*/
/*| . . . . . . . X . . . . . . . . |*/  0x00,0x00,
/*| . . . . . . X X X . . . . . . . |*/  0x00,0x00,
/*| . . . . . . X X X . . . . . . . |*/  0x00,0x00,
/*| . . . . . X X X X X . . . . . . |*/  0x00,0x00,
/*| . . . . . X X X X X . . . . . . |*/  0x00,0x00,
/*| . . . . X X X X X X X . . . . . |*/  0x00,0x00,
/*| . . . . X X X X X X X . . . . . |*/  0x00,0x00,
/*| . . . X X X X X X X X X . . . . |*/  0x00,0x00,
/*| . . . . X X X X X X X . . . . . |*/  0x00,0x00,
/*| . . . . X X X X X X X . . . . . |*/  0x00,0x00,
/*| . . . . . X X X X X . . . . . . |*/  0x00,0x00,
/*| . . . . . X X X X X . . . . . . |*/  0x00,0x00,
/*| . . . . . . X X X . . . . . . . |*/  0x00,0x00,
/*| . . . . . . X X X . . . . . . . |*/  0x00,0x00,
/*| . . . . . . . X . . . . . . . . |*/  0x00,0x00,
```

이후 이 값의 16진수 변환 값을 계산하여 저장한다

```c
//135 diamond suit
/*| 8 4 2 1 8 4 2 1 8 4 2 1 8 4 2 1 |*/
/*| . . . . . . . X . . . . . . . . |*/  0x01,0x00,
/*| . . . . . . X X X . . . . . . . |*/  0x03,0x80,
/*| . . . . . . X X X . . . . . . . |*/  0x03,0x80,
/*| . . . . . X X X X X . . . . . . |*/  0x07,0xc0,
/*| . . . . . X X X X X . . . . . . |*/  0x07,0xc0,
/*| . . . . X X X X X X X . . . . . |*/  0x0f,0xe0,
/*| . . . . X X X X X X X . . . . . |*/  0x0f,0xe0,
/*| . . . X X X X X X X X X . . . . |*/  0x1f,0xf0,
/*| . . . . X X X X X X X . . . . . |*/  0x0f,0xe0,
/*| . . . . X X X X X X X . . . . . |*/  0x0f,0xe0,
/*| . . . . . X X X X X . . . . . . |*/  0x07,0xc0,
/*| . . . . . X X X X X . . . . . . |*/  0x07,0xc0,
/*| . . . . . . X X X . . . . . . . |*/  0x03,0x80,
/*| . . . . . . X X X . . . . . . . |*/  0x03,0x80,
/*| . . . . . . . X . . . . . . . . |*/  0x01,0x00,
```

그림의 HEX값을 계산했다면 아래로 내려서 glyph information table 에 glyph 정보를 추가하기

```
  {   976, 16, 3, 21, 2,- 2}, //134 space bar
  {   982, 16,15, 21, 3,-16}, //135 diamond suit
  {     0, 16,21, 21, 3,-19}, //136
```

- Index값은 마지막에 있었던 "space bar"의 index 976에, 3줄, 6byte를 추가한 값 982를 적어줌
- width, height : Width는 16으로 고정, Height는 15줄 작성했으므로 15를 적어줌
- xAdv값은 항상 21. 
- dX, dY : 3, 16으로 먼저 테스트해봄. 이 값은 직접 해보면서 맞춰볼 것
- ♥, ♣, ♠ 모양도 추가해볼 것

이후 마지막에 해당 문자 정의 추가

```c
#define MY_SPACE_BAR 134 
#define MY_DIAMOND_SUIT 135 
#define MY_HEARTS_SUIT 136 
#define MY_CLUB_SUIT 137 
#define MY_SPADE_SUIT 138
```

![KakaoTalk_20240620_161605927_03](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/05896b55-51a1-4bab-9ff2-9f089136a3d6)

문자를 추가하고 출력해본 모습

![KakaoTalk_20240620_161605927_02](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/fd8ef5aa-7435-4465-b822-6606083cb60f)

확대도 잘 된다

## How to display your symbols
---
- FreeMono18pt와 SymbolMono18pt의 전환을 쉽게 하기 위해 이런 방법을 사용함
- 0-31 문자를 사용했으나, 확장성 때문에 127이상 글자를 선택함

```c
/*
 * Use this function instead of display.drawChar to draw the symbol or to use
 * the default font if it's not in the symbol range.
 */
void drawSymbol(uint16_t x, uint16_t y, uint8_t c, uint16_t color, uint16_t bg, uint8_t Size){
  if( (c>=32) && (c<=126) ){ //If it's 33-126 then use standard mono 18 font
      display.setFont(&FreeMono18pt7b);
  } else {
    display.setFont(&SymbolMono18pt7b);//Otherwise use special symbol font
    if (c>126) {      //Remap anything above 126 to be in the range 32 and upwards
      c-=(127-32);
    }
  }
  display.drawChar(x,y,c,color,bg,Size);
}
```
- `display.drawChar(x,y,c,color,bg,Size) `함수를 이용해 원하는 문자를 출력할 수 있음
	- 이러한 스타일의 폰트를 사용 시, Adafruit GFX 문서에 설명된 것 처럼 배경색이 무시될 수 있음
	- `drawSymbol(0,20, MY_HEART_SUIT,ILI9341_RED,0,2);` 과 같이 배경색 설정 가능
	- `drawSymbol(0, 20, MY_DIAMOND_SUIT, ST7735_WHITE, ST7735_BLACK, 2);`

![KakaoTalk_20240620_161605927](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/4c613f24-8fe5-4f69-ba3c-b7a97df7849b)

원하는 그림을 `drawSymbol` 함수로 손쉽게 출력 가능

## Advanced topics
---
- 16픽셀 고정 폰트가 아닌 다른 사이즈 폰트 사용 방법
- 8픽셀,  12픽셀 등 너비의 문자 정의 방법 예시

