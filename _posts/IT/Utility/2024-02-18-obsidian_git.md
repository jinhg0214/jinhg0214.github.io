---
title: "Obsidian Git 플러그인 사용법"
date: 2024-02-18 22:10:10 +0900
categories: [IT, Utility]  # 최대 2개 가능
tags: [obsidian, git, commit, push]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://upload.wikimedia.org/wikipedia/commons/thumb/1/10/2023_Obsidian_logo.svg/800px-2023_Obsidian_logo.svg.png"
    alt: 
---

깃페이지를 개설하면서, 옵시디언을 같이 시작했는데

어느순간 옵시디언의 플러그인인 Obsidian Git이 작동을 멈췄었다

백업용으로 사용하고있었는데, 미리 확인해서 다행

아마 최근 컴퓨터를 포멧하면서, 깃 세팅이 꼬여 커밋, 푸시가 이루어지지 않고 있었던 것


# 1. 옵시디언을 깃으로 관리하면 뭐가 좋은데?
---

[obsidian](https://i.namu.wiki/i/72c4FlVzx76QWVQXfyoUEj319LEJ_5T2BZSVMIXyPeTufPFcZdNFHY1SsfHxOxci--XlR9ASIvCXIgmbzpiyFIfD3OAC03yqSUMV6_N62sCfxfJ0XQMYhZcShMw8kvfB-s1zpkP4WVaupz5LbgNL6g.svg)

- 옵시디언은 로컬 기반 마크다운 문서 노트 앱

- 로컬 기반이기 때문에, 자동 동기화 기능을 사용하려면 유료 결제 필요

- Git으로 관리하면 어느정도 동기화 기능을 사용할 수 있다

- 또한, 오토커밋을 통해 백업을 구축할 수 있음

- 자동으로 잔디를 심을 수 있음(중요)

# 2. 옵시디언 깃 세팅법
---
완전 초기 세팅이라면 1번부터,

나처럼 포멧 후 재설치라면 2번부터 진행하면 됨

### 1. 깃헙에 원격 repository 만들기

![화면 캡처 2024-02-18 221923](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ee5c468b-8a89-43e1-870c-e03c52c49361)

1. 레포 이름 설정. 원하는 이름 아무거나
2. 설명 설정. 생략해도 되나 레포 많아지면 햇갈리니 작성 추천
3. 공개 범위 설정. 옵시디언에 개인적인 내용이 많아 Private로 설정했음
4. README.md 파일 생성. 옵시디언 초기 세팅등 적어두면 편해서 작성 추천
5. 레포 생성

### 2. Git clone 으로 원격 저장소 내려받기

[![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ecac6e9c-2b61-4180-8a1d-efea2e4f884d)](https://git-scm.com/)

Git이 설치되어있다는 가정하에 진행

원하는 폴더에 생성한 레포 주소 복사해서 git clone 하기

### 3. Obsidian Git 설치 후 연동

![화면 캡처 2024-02-18 222717](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/2ca78085-bde8-4fd8-833d-7afc3b6e3982)

옵시디언 실행 후 보관소 폴더 열기 선택

Git clone한 레포지토리를 연다

언어 설정도 원한다면 한국어로 바꾸기. 한글화가 잘 되어있다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/68462f9a-775f-4e3d-8797-158ccf235003)

새 창이 열렸다면, 좌측 하단의 톱니바퀴 버튼 눌러 설정 열기

![화면 캡처 2024-02-18 223133](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f5f4b220-691e-4c89-989a-f4f2b6726f7d)

옵션 > 커뮤니티 플러그인 탭에 들어가서 

커뮤니티 플러그인 사용 체크

이후 커뮤니티 플러그인 탐색

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/cb268d19-e657-43f2-a9e1-1095d82811ef)

Git 검색 후 설치. 77만명이나 받았다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ae7ef229-727f-4900-9b55-d1fe2f77f7c2)

활성화 체크

이후 옵션에 들어가면

![화면 캡처 2024-02-18 223451](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/fbd4534b-ceaa-4d81-a840-d88bfbc6b716)

다음과 같은 설정들이 있는데 중요 설정들부터 살펴보면

1. Split automatic commit and push : 자동 커밋 활성화 여부
2. Valut commit interval(minutes) : 몇분마다 자동 커밋할 것인지(분단위)
3. Auto Backup after stop editing any file :    
   파일 수정이 끝난 기점으로 분을 셀 것인지, 체크 안하면 무조건 시간 주기로 커밋함
4. Valut push interval(minutes) : 몇분마다 레포에 푸시할 것인지(분단위)

--- 

이정도만 세팅해도 백업 걱정은 없다 

끝