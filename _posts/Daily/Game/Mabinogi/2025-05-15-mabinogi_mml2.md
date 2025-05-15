---
title: "마비노기 모바일 : MML 작곡하기"
date: 2025-05-15 23:22:00 +0900
categories: [Game, Mabinogi]  
tags: [score, music, mml, nwc, 3mle, mabiicco]    
toc: true
comment: false
published: true
image:
    path: "https://fourthline.jp/mabiicco/mabiicco_ss3.png"
---

## 2. 직접 MML 악보 작성하기

![image](https://www.gamechosun.co.kr/dataroom/article/20250326/212784/130874_1742991033.jpg)

원하는 노래의 악보가 없는경우, 직접 작성해서 악보를 만들어야 한다

크게 다음과 같은 과정으로 이루어진다

```
1. 원하는 음악의 MIDI 혹은 PDF을 구하거나, 만든 뒤
2. 이를 MML 프로그램에 넣어 변환 및 수정
3. MML 코드를 인게임에서 플레이 후 확인, 피드백
```

---

### 2-1. 악보를 제작하여 MIDI나 PDF 파일로 저장

MIDI(Musical Instrument Digital Interface) 파일을 제작할 수 있는 작곡 프로그램들을 사용하면 된다

FL Studio, Ableton Live 같은 전문적인 프로그램부터, 

MusicScore, LMMS 와 같은 무료, 경량 프로그램 등 다양한 선택지가 존재한다

![nwc](https://noteworthycomposer.com/img/getting-started.png)

- NoteWorthyComposer는 NoteWorthy Software에서 개발한 MIDI 기반의 사보 프로그램
- 악보를 직접 만들어 미디 파일이나 가라오케 파일로 변환이 가능하다
- 사보 프로그램중에서도 사용법이 간단하고 접근성이 좋아 인기가 많으며 값이 싼편
- 구글에 NoteWorthy Composer 튜토리얼, 강좌 등으로 검색하면 다양한 자료가 나온다

![musicscore](https://musescore.org/themes/musescore_theme/images/frontpage/laptop/laptop_desktop_1x.png?cache-v1)

- MusicScore사에서 제공하는 MusicScore 
- 오픈소스이며 윈도우, 맥, 리눅스 등을 지원하며 미디파일을 지원한다
- [MuseScore 초보자 가이드 (한글)](https://musescore.org/ko/handbook)  
  
이 외에도 여러 선택지들이 있으니 원하는 프로그램을 골라 MIDI 파일을 만들면 된다

---

### 2-2. MML 작곡 프로그램을 이용해 MML로 변환

MML 작곡 프로그램으로는 2025년 기준 두가지 프로그램이 사용된다

- 3MLE 
	- Mabinogi Music Macro Language Editor 줄여서 MMMLE, 3MLE라고 한다
	- 2008년 이후 업데이트가 지원이 중단되었지만, 꾸준히 사용되고 있다
- Mabiicco
	- たんらる(@[fourthline](https://x.com/fourthline)) 라는 유저가 제작한 프로그램
	- https://fourthline.jp/ 에서 다운로드 및 버전 업데이트 가능하며 현재도 꾸준히 업데이트 중이다
	- 초보자도 쉽게 사용할 수 있고, 무료라는 점 덕분에 많이 사용하고 있다
- 두 프로그램의 차이점이라면, 3MLE는 타이핑 작업, Mabiicco는 마우스 클릭 작업이라는 점이 있다

![mmllab](https://blog.kakaocdn.net/dn/2OOBP/btszLWPOZ7L/bprCKrHVkbA8SZY6B67ZX0/img.png)

- 마비꼬에 관한 내용들은 [마비노기 MML 연구소](https://www.mabicompose.com/home)가 있으므로 링크를 남김 
	- [미디로 악보 만들기 1편](https://www.mabicompose.com/lecture04/midi_1), [2편](https://www.mabicompose.com/lecture04/midi_2)을 보면 자세한 설명이 제공되어있음

#### MML 작곡 시 유의할 점

- 마비노기 모바일은 일부 악기 음역대가 제한되어 있어, 변환 후 음이 누락될 수 있음
	- 소리가 나지 않거나, 타이밍이 안맞아 음이 밀리는 등의 문제가 발생 할 수 있음
	- 그러므로, 인게임에서 직접 미리듣기를 통해 확인 후 이를 재 수정하는 과정이 필요함
- 되도록 간단한 단선율부터 시작해 점진적으로 다음 트랙 작업으로 넘어가는 것을 추천