---
title: "헬다이버즈2 스트라타젬 프로젝트"
date: 2024-04-24 17:30:00 +0900
categories: [IT, Embedded]  # 최대 2개 가능
tags: [helldivers, arduino, keyboard]    
toc: true
comment: false
published: true
image:
    path: "https://cdn.mos.cms.futurecdn.net/pGGx5QZVgEeRgczYMgj9hX-1200-80.png" 
    alt: 
---

## 1. 서론
---

요즘 헬다이버즈2라는 TPS 게임을 하는데, 

다른게임에서 버튼 하나만 딸깍 누르면 스킬을 사용하는 방식과는 다르게,

스트라타젬(Stratagem)이라는 특수한  커맨드를 통해 보급, 무기, 궤도 포격 등을 요청할 수 있다

←↑→↓, 즉 WASD를 눌러 올바른 커맨드를 입력해야 스트라타젬을 호출할 수 있다

![StratagemSTEAM](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/5c7702c8-0c8f-46ef-b4b9-399668b352ed)

![stratagem hero](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/3810dbde-f277-430c-867b-6f1e5f7dd4ad)

이런 식으로 WASD 커맨드를 눌러, 지원을 호출한다

이게 평소에는 할만한데, 벌레 수십 마리한테 쫓기면서 입력하다보면 

상당히 손가락도 꼬이고, 조금만 실수하면 벌레들이 나를 씹고 뜯고 맛보고 즐게된다

![pizza](https://static1.srcdn.com/wordpress/wp-content/uploads/2024/03/helldiver-soldier-terminids-from-helldivers-2-1.jpg)

유튜브를 보다가 아래 영상을 접하게 됬는데

![Honeycam 2024-04-24 14-18-18](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f9b47448-2c0b-4d5f-be4b-eb7537f423eb)

[https://www.youtube.com/watch?v=FZdmfGTkVa0](https://www.youtube.com/watch?v=FZdmfGTkVa0)

스트림 덱이라는 20만원짜리 방송용 장비에

각 버튼에 해당 스트라타젬을 매핑해서 사용하는 영상이였다

이정도면 나도 만들 수 있겠는데? 싶어서 도전해보았다


## 2. 본론
---

### 2-1. 기존 프로젝트 분석

Elgato Stream Deck이라는 스트림 덱을 사용하여, 

Elgato 사에서 제공하는 소프트웨어를 이용해 매핑한 것으로 보인다

그러나, 헬다이버즈 하자고 20만원짜리 스트림덱을 사자니 배보다 배꼽이 더 커 보이므로, 

직접 구현해보기로 했다   

결국 `버튼을 누르면 -> 해당 매크로를 실행한다` 라는 기능을 하는 하드웨어이므로

어떻게 보면 매크로 키보드와 같다고 볼 수 있다고 생각했다

![71xBiCgCkIL](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/0b876f53-8677-4a6f-b93e-2274217a924e){: width="300"}

매크로 키보드란 이런 키패드같이 생긴 작은 키보드인데,

해당 키를 입력하면 사전에 등록해두었던 작업이 실행되는 키보드다

게임이나 업무용으로도 쓰는 사람들이 꽤 있다고 함

### 2-2. 개발 보드 선정

집에 사용하지 않는 ESP32, 아두이노, 라즈베리파이가 있었는데

이 중에서 아두이노 우노 R3로 선정했다.

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ec0a96fa-02fd-494b-bab5-d3292999702c)

아두이노로 선정한 이유는, 

ESP32보다 처리 능력이 높아 복잡한 매크로 입력도 수행할 수 있고,

라즈베리파이는 컴퓨팅 성능이 너무 과했기 때문에 그 중간인 아두이노로 선정했다.

이 외에는 버튼, 빵판, 저항 같은 자잘한 부품들이 필요했다

### 2-3. 소프트웨어 개발

아두이노 UNO R3로 키보드 예제를 돌려보다가

`keyboard.h` 라이브러리 사용 불가 에러가 발생했다

아두이노 UNO R3는 2011년에 발매되어 `ATmega16U2` 칩셋을 사용하는데, 

해당 라이브러리는 그 이후 버전인 `32u4` 및 `SAMD based boards`만 지원한다고 한다

결국 추가 지출이 필요한건가 고민하면서 더 뒤져보다가, UNO R3에서 키보드 돌리는 방법을 발견했다

<iframe width="560" height="315" src="https://www.youtube.com/embed/tvqA-JcTQNg?si=fYz4bhi6qn5_Lzp8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/j05vj8zRP1o?si=tHLNkMlo6GZvF7fD" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

자세한 방법은 해당 영상을 참고

펌웨어 다운로드도 해당 영상의 링크에 있음

펌웨어를 덮어씌우기 때문에, 잘못하면 우노가 벽돌이 될 수 도 있으므로 주의!!!!

위 영상들을 간단히 설명하면, 우노의 펌웨어를 키보드 HID로 덮어씌워 키보드로 인식하게 하고,

UART 전송으로 키 입력을 보내는 방법이다

##### 아두이노를 키보드로 바꾸는 방법

```
1. 아두이노 IDE에서 해당 프로그램을 빌드 후 적재한다
2. ICPS2 핀을 쇼트 시켜 DFU 모드로 진입한다
3. Flip 프로그램을 실행시켜, 아두이노 키보드 펌웨어를 적재한다
4. 이후 USB를 뺏다 꼽으면, HID 인터페이스로 인식하게 된다
```

그러나, HID 인터페이스로 인식한 상태에서는 

아두이노 IDE에서 아두이노를 인식하지 못하여 프로그램 수정이 불가능하기 때문에,

다시 아두이노 순정 펌웨어를 적재시켜줘야 한다.

##### 아두이노 펌웨어를 다시 적재하는 방법
```
1. HID 인터페이스 인식한 상태의 아두이노를, 마찬가지로 ICPS2핀을 쇼트시켜 DFU 모드로 진입
2. Flip 프로그램에서, 아두이노 순정 펌웨어를 적재한다
3. 이후 USB를 재인식시키면, 다시 아두이노로 인식하게 된다
```

위 두 과정을 번갈아가면서 코딩하고, 디버그 출력해보고, 키보드 테스트해보고, 

다시 코딩하고 하는 과정을 반복했다

상당히 번거로운 방법이지만, 그래도 키보드로 쓸 방법이 있다는 것에 만족하고 사용함


##### 소스코드

[https://github.com/jinhg0214/Helldivers2](https://github.com/jinhg0214/Helldivers2)

해당 레포지토리 참고

UART 통신을 이용해, `buf[2]`에 keycode를 전달해주는 것이 핵심

버튼을 눌렀을 때는 키코드를 보내지만, 떼었을 때 release 함수를 통해 `00000000`를 보내주어야 제대로 인식한다

### 2-4. 하드웨어 세팅

![회로도](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/9bed1598-8790-4b69-8040-c903062b3122)

각 버튼을 풀업버튼으로 구성 후, 병렬적으로 쭉 연결해주면 된다

버튼 인식을 한 번만 하면 되기 때문에 풀업 풀다운 중 하나를 선택해야 하는데, 

아두이노에서는 핀모드에 풀다운 모드가 없어서 풀업 방식을 선택했다

각 버튼을 눌렀다 뗐을 때, 입력이 잘 들어가는지 체크해보면 된다

![IMG_3761](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ca924027-1cae-47c7-9727-a30afeaf23dc)


### 2-5. 결과 화면

![Honeycam 2024-04-24 18-01-38](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f26a1711-f0de-4133-b922-cc0cfeca7bfb)


버튼을 눌렀을 때, 해당 스트라타젬이 호출되는 모습

잘 된 다!


## 3. 결론
---

### 3-1. 후기

실제로 인게임에서 사용해 본 결과

오히려 정신없는 상황에서는 키보드와 마우스에서 손을 뗄 수 없었기 때문에

시작지점과 같은 여유 있는 상황에서밖에 사용할 수 없었다

실사용은 아쉬웠지만 다양한 것들을 배울 수 있었다

- 아두이노 펌웨어를 덮어씌워서 HID로 인식하는 방법
- UART 통신을 이용한 MCU - PC 간의 통신
- keycode를 이용한 다중 키 입력 제어 

### 3-2. 개선할 점

#### 손으로 안되면 발로? 

손으로 버튼을 눌러야 하므로, 긴박한 상황에서 사용이 제한되었다  

![61Fo6Od6MQL](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/6cc45861-7386-4ba7-a824-751d5c9ea1c8){: width="300"}

차라리 페달 방식으로 바꿔서 발로 누르는것도 괜찮을듯 싶다

#### 코드 개선

##### 아예 ctrl 키를 누를 필요 없도록 제어하기   

`Serial.write()` 신호를 보낼 때, Ctrl을 동시에 누른 신호를 보낼 수 있도록 수정하기

##### 스트라타젬을 문자열로 하드코딩해놨는데, 만약 업데이트로 추가된다면 펌웨어를 또 업데이트 해야 함   

이를 보완할 수 있다면 좋을듯

#### 하드웨어

배선이 노출되어있어, 고양이가 자꾸 선을 가지고 놀다가 빠지는 경우가 발생   

3D 프린터를 이용해 케이스를 씌우면 좋겠다는 생각이 들었으나

추가 비용이 발생할 것으로 예상되어 포기

#### 손목형 스트라타젬 호출기   

해당 프로젝트의 목표는

긴박한 상황에서 스트라타젬의 정확도를 높이는 프로젝트였으므로, 

약간 다른 이야기지만 아래 영상을 보게됬는데, 

![Honeycam 2024-04-24 13-49-26](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/18fca4f3-6d73-45ee-97dd-69ea3cf34b03)

[https://youtube.com/shorts/etx6uggk3aA?si=JcARt6UPtdPdktlt](https://youtube.com/shorts/etx6uggk3aA?si=JcARt6UPtdPdktlt)

터치패드를 이용해 암밴드를 구현하여 직접 손목에 차고 플레이할 수 있게 했다

개발자 말로는 그다지 실용적이진 않지만, 상당히 재밌다고 한다

[개발자 인스타그램](https://www.instagram.com/sudomod_wermy/?hl=en)에 올라온 정보를 토대로 확인해보면

3.5인치 TFT SPI 터치스크린을 직접 구현한 GUI를 통해 사용하는 것 같았고,

arduino/esp32와 리튬 배터리를 사용하는 것 같았다

그 외에 직접 소리도 출력하게 스피커도 장착한 것으로 보인다

하드웨어 제작 프로젝트를 하게 된다면 도전해볼 만 할 듯

끝.