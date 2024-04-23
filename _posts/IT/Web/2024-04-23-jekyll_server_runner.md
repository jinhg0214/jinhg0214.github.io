---
title: "Jekyll 서버 자동 실행 bat 파일 만들기"
date: 2024-04-23 10:00:00 +0900
categories: [IT, web]  # 최대 2개 가능
tags: [jekyll, windows, bat]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://static1.makeuseofimages.com/wordpress/wp-content/uploads/2015/06/windows-batch-files.jpg"
    alt: 
---

## 귀칞음

깃 블로그를 운영하면서

보통은 귀찮아서 바로 서버에 올려버리지만

사진이 많거나, 에러 터질거같다 싶으면 로컬에서 확인해야했다

그러나 서버를 키는데만 해도

1. 터미널 열기

2. 깃블로그 위치로 이동

3. `bundle exec jekyll serve` 명령어 입력

세가지 과정을 거쳐야 했기 때문에 귀찮아서 이것저것 알아본 결과

나처럼 귀찮음을 느낀 사람들이 있었다

## 만드는 방법

1. 아무 텍스트 파일 열어서 아래 명령어 입력 
```
cd C:\Ruby32-x64\bin 
call setrbvars.cmd
cd {자신의 깃 블로그 경로}
bundle exec jekyll serve
```
설명

`cd C:\Ruby32-x64\bin ` : 자신의 루비 버전 폴더에 맞게 수정할 것

`call setrbvars.cmd` : 해당 폴더의 `setrbvars.cmd`를 실행

`cd {자신의 깃 블로그 경로}` : 자신의 깃 블로그 경로로 이동.    

다른 드라이브에 있다면 `D:` `E:` 등과 같이 드라이브를 바꿔주고 cd 해주면됨

2. 이를 bat파일로 저장한다

ASNI 형식으로 저장해야한다는데, 그냥 저장해도 문제되지 않았음

### 참조

[Jekyll 서버를 실행시키는 batch 파일 만들기](https://wormwlrm.github.io/2018/08/13/How-to-make-a-batch-file-to-run-Jekyll-server.html)