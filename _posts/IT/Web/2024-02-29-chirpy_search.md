---
title: "Jekyll chirpy 테마 검색창 활성화 방법 (.min.js 에러)"
date: 2024-02-29 17:18:00 +0900
categories: [IT, Web]  # 최대 2개 가능
tags: [web, chirpy, deployment, search, npm, build]   # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/7405e63f-8442-4b00-bcc8-ea40e9c79e55"
    alt: 
---

포스팅을 작성할 때

로컬에서 깃 페이지 `bundle exec jekyll serve`로 실행하면

`Error “/assets/js/dist/home.min.js”` 에러가 발생했었는데

페이지는 잘 표시되서 별 생각없이 넘기고 있었다.

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/8289effb-eab5-4ccd-b238-0bca9297c994)

그러나 Chirpy 테마에서 오른쪽 위에 검색어 창에 키워드를 입력해도

검색이 되지 않는 문제가 발생하여 검색해보다

이와 연관이 있는것을 발견했다.

# 원인 : `Error “/assets/js/dist/home.min.js” 
---

![Error](https://global.discourse-cdn.com/standard14/uploads/jekyllrb/original/2X/e/e270a5a0e51725ec0c1b42ca9f0be676558aa4ff.png)
- min.js 파일 등이 존재하지 않는 오류
- chirpy 초기 설치시 `init` 명령어를 수행하면 `/assets/js/dist` 폴더에 몇가지 js 파일이 생성되어야 하는데, 생성되지 않아 발생하는 문제
- 여기 생성되는 js 파일들은 TOC를 보여주는 기능 등을 수행하는데,    
파일이 없다면 원격 저장소에서도 오류가 발생하고, 로컬에서도 문제가 발생한다고 함

# 해결 방법
--- 

1. Node.js 와 npm 설치
2. npm을 이용해 inital 및 build 
3. 원격 저장소에 push

### 1. Node.js 설치
[![nodejs](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/7405e63f-8442-4b00-bcc8-ea40e9c79e55)]((https://nodejs.org/en))

- 위 링크에서 node.js를 받아 설치
- node.js 설치하면 npm은 자동으로 설치된다

### 2. npm을 이용해 inital 및 build 

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/26ae9936-5652-4f16-ba92-507753d4e962)

- powershell 등을 열어 **깃페이지 경로로 이동 후** `npm install && npm run build` 명령어 실행

![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/7f01be2e-d2c8-469f-a2ba-d686ba81d8e8)

- `NODE_ENV` 관련 에러 발생 시 `npm install -g win-node-env` 명령어로 npm 재설치
- 이후 `npm run build` 재실행
- min.js 파일들이 생성된다

### 3. 원격 저장소에 push
![image](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/277541d8-c709-4eeb-9b06-fdb820728873)

- 원격 저장소에도 업로드하기위해 `gitignore` 파일 수정
- 기본적으로 `.min.js` 파일은 git에 ignore되어있으므로 이를 없애주어야한다
- `.gitignore`파일에 `assets/js/dist` 파일을 주석처리 후 commit, push하면 끝


참조 :   
[Theme "jekyll-theme-chirpy"](https://talk.jekyllrb.com/t/theme-jekyll-theme-chirpy/8524)

[jekyll chirpy 설치 시 발생하는 오류 해결하기](https://kiffblog.tistory.com/233)

[github pages 블로그 만들기 - 테마 적용하기(Chirpy)](https://ree31206.tistory.com/entry/github-pages-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%85%8C%EB%A7%88-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0Chirpy)

[Jekyll Chirpy 테마 사용하여 블로그 만들기](https://www.irgroup.org/posts/jekyll-chirpy/)