---
title: "ChatGPT로 유튜브 영상 요약하기"
date: 2024-07-16 13:51:00 +0900
categories: [IT, Ai]  # 최대 2개 가능
tags: [chatgpt, plugin, youtube, productivity, summary]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: false
image:
    path: "https://miro.medium.com/v2/resize:fit:1200/1*xOPEyGvaGKRja-k3eDyCMw.png"
    alt: 
---

유튜브를 보다가, 관심있는 주제의 동영상이지만

영상이 너무 길어서 빨리감기로 보다가 

그냥 예전처럼 글로 작성해주면 더 쉽게 볼 수 있을것 같은데

이걸 내가 왜 이렇게 보고 있어야하나 회의감이 들었다

다행히 이런 생각을 나만 한 것은 아닌지

유튜브 영상을 ChatGPT 요약해주는 방법들이 있다

## 1. 크롬 플러그인 사용

![image](https://github.com/user-attachments/assets/d48d389f-cb86-4eb1-8724-f22160e4dbb6)

[YouTube Summary with ChatGPT & Claude](https://chromewebstore.google.com/detail/youtube-summary-with-chat/nmmicjeknamkfloonkhhcjmomieiodli)

크롬 브라우저를 사용중이라면, 플러그인 설치를 통해 쉽게 유튜브 영상을 요약할 수 있다

![image](https://github.com/user-attachments/assets/e54239f6-cae5-4c55-8afa-1d0296fe5c36)

플러그인을 설치하면 유튜브 영상 우측 상단에 `Transcript & Summary` 라는 탭이 생기는데

이를 확장해보면 영상의 스크립트가 모두 적혀있는 것을 볼 수 있다

이를 두번째 ChatGPT 버튼을 누르면, 

5문단으로 요약해줘 라는 명령과 함께 이 스크립트를 통째로 ChatGPT에게 넘겨준다

### 장점

- 크롬 확장 플러그인을 사용하므로 쉽게 설치 가능
- 유튜브에서 바로 사용할 수 있어 매우 접근성이 좋다
- 모든 스크립트를 한눈에 볼 수 있어서 필요한 부분은 자세히 볼 수 있다
- 설정에서 프롬프트를 통해 추가 설정 가능

### 단점
- 자막 의존성이 높음
    - 자동 생성된 자막의 품질이 낮으면, 요약의 품질이 떨어짐
- 크롬에 플러그인을 설치해야하므로 무거워질 수 있음
- 유튜브 화면이 지저분해짐

## 2. ChatGPT 플러그인 사용

ChatGPT에서도 여러 플러그인들을 제공하기 시작했다

ChatGPT의 플러그인에 대한 내용은 추후에 자세히 다루기로 하고 일단 어떻게 사용하는지 알아보자

![image](https://github.com/user-attachments/assets/d0271e2a-979f-410d-a929-ee68d2130dd0)

ChatGPT의 좌측 상단에 보면 `GPT 탐색`이라는 탭이 있다 

![image](https://github.com/user-attachments/assets/2c70d52d-5a75-43c7-92a4-d49b827f729f)

이곳에서 다양한 플러그인들을 찾아 볼 수 있는데 여기서 

Video Insights: Summaries/Transcription/Vision 를 검색

![image](https://github.com/user-attachments/assets/3c1a4ed1-0b07-425d-9eca-680220680004)

하단의 채팅 시작을 누르면 플러그인을 사용할 수 있다

![image](https://github.com/user-attachments/assets/73f5150d-f5e5-4ae6-9d7b-f06ba7cfe2be)

이후 Video Insight라는 새로운 탭이 생기는데

여기에 기존의 ChatGPT를 사용하듯이 

```
https://www.youtube.com/watch?v=KiB0vRi2wlc&list=WL&index=40&t=105s&ab_channel=TheCherno

다음 내용을 한국어로 요약해주세요
```
영상의 링크와 함께 명령을 전송하면 된다

YouTube Summary with ChatGPT & Claude과 마찬가지로 내부에서 자막을 추출하여 이를 바탕으로 요약을 진행한다

### 장점
- ChatGPT에서 사용 가능. 크롬이나 유튜브가 지저분해지는걸 원하지 않는다면 추천
- 요약 기능뿐만 아니라 다양한 비디오 콘텐츠 분석 기능 제공
    - 영상의 키워드 텍스트 추출, 감정 분석, 주제 분석 등도 가능하다고 함 

### 단점
- ChatGPT 사용량이 플러그인보다 많으므로 무료로 사용중이라면 금방 한도에 도달함

---

몇년전만 해도 ChatGPT는 럭키 심심이였는데, 이제는 벌써 일상생활에서 다양한 용도로 사용하고 있다

얼마나 더 발전할 수 있을지 궁금하다