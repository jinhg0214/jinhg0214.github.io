---
title: "이미지 속 숨은 비밀: 스테가노그래피"
date: 2025-06-10 23:00:00 +0900
categories: [IT, etc]  # 최대 2개 가능
tags: [steganography, cybersecurity, datahiding, digitalforensics]     
toc: true
comment: false
published: true
image:
    path: "https://miro.medium.com/v2/resize:fit:1200/1*dQyfOpFWmSxrmdOcQgW6OQ.jpeg"
    alt: 
---

인터넷을 하다보면, 서로 여러 자료들을 공유하기 마련이다.

그런데 어떤 사이트에서는 이미지 파일을 제외한 다른 파일을 업로드하는 것을 금지한 곳도 있다.

이런 사이트에서 유저들은 다양한 방법으로 자료를 공유하는데,

"첨부파일 받은 뒤 확장자명 ZIP으로 바꿔서 압축 풀어보세요"

라는 게시물도 종종 올라온다.

겉보기에는 평범한 이미지 파일처럼 보이지만, 실제로는 그 안에 다른 파일이나 메시지가 숨겨져 있는 것이다.

이처럼 이미지에 다른 파일을 넣는 것을 **스테가노그래피(Steganography)**라고 하는데,

이에 대해 설명해보고자 한다.

---

## 1. 스테가노그래피란

스테가노그래피(Steganography)의 어원은 덮혀진, 혹은 숨겨진을 뜻하는 **steganós**와

쓰기를 의미하는 **-graphia**를 합친 그리스어 steganographia이다.

즉, 숨겨진 메시지를 쓴다는 뜻으로, 보이는 것과 다른 숨겨진 메시지가 있으면

스테가노그래피의 일종이라고 할 수 있다.

![vertical](https://i.namu.wiki/i/CWu1mP0t0dPCr_ORL4Nt6YQo-n8n8pxFM6dPKgaBIaSSzNJddVrgQ-o0OHLnoVWrsMrAnNgoAIzvVJqP8ysb4fWioZa_nF7q_LUGK76hAAdZylUjnTM-O7lqZKEeIxKFu__WE3DG-OO336NjES0rNw.webp)

가장 대표적인 스테가노그래피는 세로드립이다.

겉으로 보기에는 전혀 다른 내용이지만, 앞글자만 따서 읽어본다면 숨겨진 의미가 나타난다.

암호화가 내용을 읽을 수 없게 만드는 것이라면,

스테가노그래피는 **비밀의 존재 자체를 숨기는 것**이다.

---

## 2. 스테가노그래피 종류

역사가 오래된 만큼 다양한 스테가노그래피가 존재한다.

### 1. 전통적 스테가노그래피

디지털 기술이 등장하기 이전에도 사람들은 은밀하게 정보를 전달하기 위해 여러 창의적인 방법을 고안했다.

![ink](https://www.thoughtco.com/thmb/U0FkpChdr3Dy04uMQAx8j7Phe4k=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/child-using-paintbrush-and-blue-ink-to-make-invisible-numbers-appear-on-paper-90116666-570e58123df78c7d9e514fb6.jpg)

- **눈에 보이지 않는 잉크**
- 레몬즙이나 우유처럼 평소에는 보이지 않지만, 열을 가하면 보이는 특수 잉크를 사용하는 방법이다.

![microdot](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b5/German_microdots_World_War_II_Mexico_Spain.jpg/1200px-German_microdots_World_War_II_Mexico_Spain.jpg)

- **미크로도트**
- 독일에서 개발된 기술로, 중요한 정보를 극도로 축소하여 점 크기로 만든 후, 일반 편지나 문서의 마침표나 구두점으로 위장하는 방법이다.
- 현미경으로만 내용을 확인할 수 있으며, 냉전 시대 첩보 활동에서 광범위하게 사용되었다.

![text](https://cdn.comparitech.com/wp-content/uploads/2019/05/steganography-4.jpg)

- **메시지를 숨긴 문장 구성**
- 정해진 규칙(앞글자만, 특정 문자의 앞이나 뒤 등)에 따라 읽으면 숨겨진 메시지가 나타나는 방법이다.

### 2. 디지털 스테가노그래피

컴퓨터 및 전산기술의 발달로 등장한 기법이다.

전통적인 암호화 방식은 공개키, 개인키와 같이 암호화되었다는 것을 알 수 있지만,

스테가노그래피 방식은 겉으로만 봐서는 일반적인 미디어이므로, 숨겨진 내용이 있다는 것을 알 수 없다.

![LSB](https://votiro.com/wp-content/uploads/Image-Steganography-Kids-at-the-Beach.png)

- **이미지 파일**
- 이미지의 픽셀 값 중, 사람이 인지하지 못할 정도의 낮은 비트(LSB : Least Significant Bit)를 조작하여 메시지를 숨긴다.
- 위의 사진처럼 마지막 비트를 조작하면 눈으로는 구분할 수 없지만, 정해진 방법으로 분석하면 메시지를 복원할 수 있다.
- 이미지파일 속에 압축 파일을 집어넣는 유틸로는 **Jpg+FileBlender** 를 많이 사용한다

![audio](https://iicybersecurity.com/wp-content/uploads/2020/04/audio-steganography.jpg)

- **오디오 파일**
- 오디오의 무음 구간이나, 특정 주파수 대역에 데이터를 삽입한다.
- 음질에 거의 영향을 주지 않기 때문에 탐지하기도 어렵다.
- 최근에는 VoIP 통신이나 스트리밍에서도 사용 사례가 보고되었다.

---

## 3. 스테가노그래피가 쓰이는 곳

### 장점 / 순기능

- **디지털 워터마킹**을 이용한 저작권 보호, 원작자 증명
- **문서 유출 경로 추적**을 이용한 기업 보안 강화
- 언론 자유 제한 지역에서의 안전한 정보 전달

### 단점 / 역기능

- 이미지에 악성코드나 바이러스 숨기기
- 테러리스트나 범죄 조직의 은밀한 소통
- 기업 기밀 유출 및 산업 스파이 활동
- 불법 콘텐츠 유통 경로로 악용

---

## 4. 탐지 방법

스테가노그래피를 탐지하는 방법에는 여러 가지가 있다.

**기본적인 탐지 방법:**

- 원본 파일과 데이터 크기 비교
- HxD 같은 헥스 에디터로 파일 끝 이후의 숨겨진 데이터 확인
- 파일 헤더와 실제 내용의 불일치 점검

**고급 탐지 기법:**

- 통계적 분석을 통한 픽셀 값 분포 이상 감지
- Steganalysis 전용 도구 사용

악용 사례를 막기 위해 **컨텐츠 위협 제거 방법(Content Threat Removal)**을 사용하기도 한다.

이는 파일을 재인코딩하여 숨겨진 메시지를 파괴하는 방법으로, 

JPG 파일을 고압축으로 재압축하면 LSB에 숨겨진 데이터가 손실된다.

이것이 바로 일부 사이트에서 "원본으로 받아야 하는 이유"이기도 하다.

## 5. 마치며

스테가노그래피는 "가장 좋은 숨김은 숨겨진 것조차 모르게 하는 것"이라는 철학을 바탕으로 한다.

이 기술은 그 자체로는 중립적이지만, 사용하는 목적에 따라 약이 될 수도, 독이 될 수도 있다 

저작권 보호나 정보 보안 강화 같은 선한 목적으로 사용될 수도 있지만, 악성코드 유포나 불법 통신의 도구로 악용될 수도 있다.

디지털 시대에 살고 있는 우리에게 스테가노그래피는 정보 보안의 중요성을 일깨워주는 흥미로운 기술이다.

혹시 당신이 받은 그 평범해 보이는 이미지 파일도, 다시 한번 자세히 들여다볼 필요가 있을지도 모르겠다.
