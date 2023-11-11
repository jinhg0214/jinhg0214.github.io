---
title: "깃 페이지 초기 세팅 복원 방법"
date: 2023-11-11 22:43:00 +0900
# last_modified_at: 
categories: [IT, WEB] # 최대 2개 가능
tags: [gitpage, format, initialize]    
toc: true
comment: false
published: true
---

그래픽카드를 AS받는겸 SSD를 하나 부착하면서

포멧을 했더니 기존 저장된 세팅들이 다 날아가서

복원하는데 꼬박 이틀이 날아갔다

먼저 깃 페이지 글 쓸수 있는 환경을 먼저 복원하기 위한 환경을 구축한다

# 1. 루비 설치

[https://rubyinstaller.org/](https://rubyinstaller.org/)

루비 인스톨러 설치

`WITH DEVKIT` 버전으로 최신버전으로 설치한다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/84894345-d71e-45e3-9d25-d29ede9ba66f)

모든 유저용으로 설치 

동의해주고 쭉쭉 NEXT 눌러준다

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ab597e59-fc07-4aba-b20c-c7f297824f22)

`Run ridk install to set UP MSYS2 and development toolchain` 선택 후 종료
 
![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/6861a8a4-9551-4be9-86a9-7faa0e735a1d)

다음과 같은 콘솔창이 뜨면 3번을 입력하여 `MSYS2 and MINGW development toolchain`를 설치한다

설치 완료되면 끝

# 2. Jekyll 및 bundle 설치 

`window + X`키로 윈도우 파워쉘 관리자 모드로 실행

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/ccea4adc-7e5f-4a32-a53a-d85650140e2d)

`ruby -v`로 루비가 잘 설치되어있는지 확인

`gem install jekyll bundler`로 Jekyll 과 bundler를 설치한다

### Jekyll
심플한 정적 웹 페이지 생성기. HTML / Markdown 등의 언어로 글을 작성하면, 

이것을 미리 정해놓은 규칙에 따라 다양한 레이아웃으로 포장하여 정적 웹사이트를 만들어줌

### Bundler
필요한 정확한 GEM과 버전을 추적하고 설치하여 루비 프로젝트를 위한 일관된 환경 제공을 위한 프로그램

GEM : 다른 언어에서 라이브러리 개념

프로젝트를 생성하면 Gemfile이라는 Gem들이 정의된 파일이 생성되는데

Bundler로 라이브러리의 의존성을 관리함

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/19893e65-f0b6-4026-bce2-aecd3508c67d)

번들러까지 설치가 완료되었다면 끝

# 3. 의존성 gem 설치

기존에 세팅해두었던 블로그 폴더로 이동하여

`bundle install`로 gem 의존파일들을 모두 설치

이후 `bundle exec jekyll serve`로 실행하면 로컬 환경에서 깃페이지 확인 가능하다