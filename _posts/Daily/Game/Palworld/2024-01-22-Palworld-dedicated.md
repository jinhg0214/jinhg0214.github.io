---
title: "팔월드 데디케이트 서버 하마치로 만들기"
# excerpt : 요약
date: 2024-01-22 18:00:00 +0900
# last_modified_at: 
categories: [Game, Palworld] # 최대 2개 가능
tags: [window, palworld, dedicated, server, hamachi, portforwarding]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://upload.wikimedia.org/wikipedia/en/5/53/Palworld_cover_art.jpg"
    alt: 
---

팔월드(Palworld)가 24년 1월 19일 발매한지 3일만에 판매량 300만장을 돌파했다

포켓몬과 Ark, 젤다 등등 여러 게임을 짬뽕한것 같은 오픈월드, TPS, 샌드박스, 서바이벌 게임인데

최근 친구들과 재미있게 하고있다

그러나 얼리억세스 버전 0.1이라 멀티 인원이 최대 4인으로 고정되어있어,

5인 이상부터는 전용 서버인 데디케이트 서버를 직접 파야 가능하다

데디케이트 서버는 최대 32인 까지 가능

이 글에서는 윈도우 환경에서 데디케이터 서버를 여는 방법에 대해 다루겠다

그 외의 환경에서 서버를 열고자 하는 경우 공식 문서를 참조할 것

[https://tech.palworldgame.com/dedicated-server-guide#windows](https://tech.palworldgame.com/dedicated-server-guide#windows)

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/a9d6dda2-f8b6-4b73-9002-8c537b6ddf73)

데디케이트 서버의 권장 사양은

CPU : 4코어 이상

RAM : 최소 16GB, 32GB 이상 추천. 서버만 돌리면 8GB인데 플레이까지 하려면 더 필요

Network : UDP 포트 8211 포트포워딩 필요 (아래 추가로 설명)

# 세줄요약
---
1. Palworld 데디케이트 서버 프로그램 
2. 포트포워딩
3. 포트포워딩 실패 시, 하마치 이용

# 1. Palworld 데디케이트 서버 프로그램 

---

### 1) 데디케이트 서버 프로그램 다운로드

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/96b2af85-998c-4751-abb5-e1ab3eccfa60)

스팀에서 팔월드를 구매했다면, 데디케이트 서버 프로그램도 자동으로 라이브러리에 추가되어있다

스팀 라이브러리에서 왼쪽 탭에 보면 검색 범주가 게임으로 되어있는데, 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/2b545acd-b5c3-482b-876b-6d9258e0a960)

소프트웨어를 체크 해주고, `palworld` 를 검색하면 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/dffbd963-b2a8-458a-8f72-6395998af1cb)

데디케이트 서버가 검색된다

### 2) 데디케이트 서버 프로그램 실행

이 파일을 설치 후, 실행

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/0f12ce8e-cec8-4ef2-b7d4-afa936145441)

다음과 같이 뜨는데

- Palworld Dedicated Server 플레이 : 비공개 서버
- Open and start as a community server : 리스트에 뜨는 커뮤니티 서버로 시작

친구들과 초대받은 사람들만 사용하기 위해 위 체크 후 플레이

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/9967b5b0-a378-46b7-b468-e4487703c65c)

다음과 같이 뜨면 서버 프로그램은 끝

### 3) 서버 동작 확인

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/657a2609-fb78-41d9-b258-659364ef0077)


이후 팔월드 게임을 실행 후, 하단에 자기 자신을 접근하는 IP인 

localhost `127.0.0.1:8211` 연결을 눌러서 접속 테스트 확인

캐릭 생성화면까지 뜬다면 성공


# 2. 포트포워딩

---

포트포워딩은 통신사마다, 공유기 모델마다 다르기 떄문에 이 글에서는 다루지 않음

간단히 설명하면 포트포워딩을 통해 `8211` 포트를 개방해 주어야 접속이 가능하다

[https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/)

위 사이트에서 포트가 개방되었는지 테스트 가능

아래 리스트들을 체크해보고 그래도 안된다면 하마치 이용

1. 포트포워딩 시도

2. 그래도 안되면 DMZ 기능 시도 (없는 통신사, 모델도 있음)

3. 그래도 안되면, 게이트웨이 모드에서 브릿지 모드로 변경


# 3. Hamachi

---

하마치란?

LogMeln 사에서 개발한 가상 사설망(VPN) 애플리케이션

직접 네트워크를 생성하고, 다른 사람을 자신의 네트워크에 초대할 수 있다

2번의 포트포워딩 방법이 인터넷을 이용하는 거라면,

하마치는 가상의 PC방을 만들어서 PC들을 가상의 네트워크로 연결하는 거라고 생각하면 쉽다

복잡한 포트포워딩 또는 DMZ 설정을 패스하고 접속할 수 있다는 장점이 있으나

속도가 떨어질 수 있다는 단점이 있다

- 하마치로 네트워크를 개설할 경우, 무료 버전으로는 최대 5인까지 가능하다

### 1) 하마치 다운로드

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/5ee117f1-4021-4a4b-991c-b162352aa7cc)

[https://vpn.net/](https://vpn.net/)

위 공식사이트 링크에서 하마치 프로그램을 받고 설치

이후 프로그램을 실행 후 가입

### 2) 가상 네트워크 만들기

![hamachi0](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f4633a38-c6af-4def-9760-9ac0b4bba3c1)

`25.X.X.X`로 시작하는 가상 IPv4와 IPv6 를 할당해준다(영국 IP라고 함)

이후 `네트워크 > 새 네트워크 만들기` 를 누르면, 새로운 네트워크를 생성할 수 있다

![hamachi1](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/4fa9a4a5-9889-4674-a120-7e7bde9df098)

네트워크 ID : 리스트에 표기될 네트워크 이름

암호 : 접속시 필요한 비밀번호

만들기 클릭하면, 새로운 네트워크가 생성된다

서버장은 하마치를 킨 상태에서, 1번의 팔월드 데디섭 프로그램을 실행시켜주면 끝

### 3) 하마치 테스트

테스트해보고싶으면 자신의 하마치 IP에 8211 포트로 접속해보면 된다

ex) 1번처럼 `127.0.0.1` 대신 `25.x.x.x:8211`로 접속. 캐릭생성창이 뜨면 OK

--- 

이후 접속하고자 하는 사용자들은 마찬가지로 하마치 설치 후 가입한 뒤 

`네트워크 > 기존 네트워크에 가입` 으로 서버장이 생성한 네트워크에 가입해주고,

서버장의 하마치 IP에 포트번호를 붙여 접속하면 된다(`25.x.x.x:8211`)

만약 하마치의 그룹에 가입이 되지 않는다면 하마치 프로그램이 방화벽에 등록되어있는지 확인해볼 것

