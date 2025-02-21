---
title: "Chirpy 7.2.4 수동 업데이트"
date: 2025-02-21 16:34:00 +0900
categories: [IT, Web]  # 최대 2개 가능
tags: [gitpage, gitaction, chirpy, jekyll, ruby, cicd]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://camo.githubusercontent.com/7deb9e4905ab1e73cec83fa80f3a5d0c7f613e6b522a9fdc41d5c79fad37eda8/68747470733a2f2f6368697270792d696d672e6e65746c6966792e6170702f636f6d6d6f6e732f646576696365732d6d6f636b75702e706e67"
    alt: 
---

Chirpy 7.2.4 버전으로 수동 업데이트하면서 겪은 경험을 정리한 글 

같은 문제를 겪는 분들에게 도움이 되길 바라면서 작성했습니다

---


2021년 처음 깃 페이지를 개설할 때

Jekyll 테마인 [Chirpy](https://chirpy.cotes.page/)를 사용했다

그때는 별다른 설치툴 없이, 깃허브에서 코드를 복사해서 내 레포지토리에 복사해서 붙여넣는 방법을 사용했었다

이 상태로 2년간 써오다가 갑자기 문제가 터졌다

## 1. 청천벽력같은 GitAction V3 서비스 종료
---

![gitaction](https://velog.velcdn.com/images/whdudtod1273/post/bb04c8fe-1004-4800-ba6a-7f2fbc1bddcb/image.jpg)

[GitHub Action, upload-artifact v3 오류 해결법](https://jinhg0214.github.io/posts/actionsv3/)

위 글에서 작성한 것 처럼

GitAction에서 2025년 1월 30일 이후로 artifact v3 actions가 서비스 중단되면서

오류가 발생하기 시작했다

자세한 기능은 해당 글을 참조

여튼 이를 해결하기 위해서는 

1. **문제되는 아티펙트만 버전업해서 사용**

2. **Chirpy 테마를 최신 버전으로 업데이트**

일단 급한대로 1번 방법을 선택해서 임시로 해결했지만,

근본적인 해결책이 아니라 찝찝함이 남았다


## 2. 새로운 Git 저장소에서 Chirpy Starter를 이용한 업데이트 시도
---

임시방편으로 아티팩트만 업데이트하여 사용하고 있었지만,

또 다른 문제가 어디서 터질지 몰라 Chirpy 자체를 최신버전으로 업데이트하여 사용하기로 결정했다

![Image](https://github.com/user-attachments/assets/1c97de05-fa06-4d61-843e-3a4264b845da)

[Chirpy 공식 문서 : Getting Started](https://chirpy.cotes.page/posts/getting-started/)페이지에

1. 스타터를 이용해서 생성하는 방법

2. 혹은 포크를 떠서 생성하는 방법

두가지 방법을 공식에서 권장하고 있다

업데이트 하는 김에 Chirpy에서 제공하는 스타터를 이용해서 깔끔하게 만들어보고자 했다

기존의 레포지토리를 _backup으로 이름을 바꾸고, 새로운 레포지토리를 개설하여 웹에서 동작을 확인했다

그러나, 이전 글을 가져오자 **모든 사진들이 깨지는 문제가 발생**했다

나는 사진을 [git issue로 이미지 첨부하는 방식](https://velog.io/@www_1216/github-issue%EB%A1%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B2%A8%EB%B6%80%ED%95%98%EA%B8%B0)을 사용하고 있었는데, 

기존 저장소의 이름을 변경하면서 이미지 경로가 모두 깨진것이였다

결국, 새로운 저장소를 만드는 방법은 포기해야했다


## 3. 기존 저장소에서 .git 제외한 모든 파일 삭제 후 최신 코드 적용
---

새로운 저장소 생성이 불가능했기 떄문에, 

기존의 저장소를 유지한 채 Chirpy 최신 버전의 코드만 적용하기로 결정

1. [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)에서 최신 소스코드를 다운로드

2. 블로그 저장소의 `.git` 폴더를 제외한 모든 파일 삭제 (꼭 백업해둘 것)

3. 최신 코드를 붙여넣고 로컬 빌드 테스트, 이후 원격 저장소에서 테스트

순서로 시도했다

로컬에서 정상적으로 작동하는 것을 확인하고, 원격저장소에 커밋했지만, **CI/CD에서 문제가 발생**했다

### `html-proofer` 검사 실패

Github Actions에서 `html-proofer`가 내부 링크와 이미지 유효성을 검사하면서 CI/CD가 실패했다

기존의 포스트 이미지들을 다 문제가 있다고 판단해버려서 글이 안올라가는 문제 발생했다

일단 급한대로, 이를 비활성화 한 뒤 빌드를 시도해봤으나 또 다른 에러들이 계속해서 발생했다

### git 커밋 실패



## 4. Jekyll 및 GitAction 학습
---

이제는 무작정 시도하는 것이 아니라, Github Actions와 Jekyll을 제대로 학습할 필요가 있었다

Chirpy가 몇년간 업데이트 되면서 초창기보다 많은 기능들이 추가되었는데

뭐가 어떤 기능을 하는지 알아야 취사선택 할 수 있기 때문이다.

[Github Action 정리](https://jinhg0214.github.io/posts/gitaction/)

이를 통해 Chirpy 테마에서 사용하지 않는 기능을 비활성화 하고, CI/CD가 정상적으로 동작하도록 설정했다


## 5. 클린 설치 후 재도전
---

혹시 환경 문제일 가능성을 고려해서 Ruby, Jekyll을 모두 삭제 후 재설치했다

![Image](https://github.com/user-attachments/assets/1951290f-74cf-4854-97db-38823ba1a369)

현재 내가 쓰고있는 버전들이다. 해당 버전들로 시도하니 문제없이 잘 동작했다

[Github 블로그 만들기](https://devpro.kr/posts/Github-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-(1)/) 

정확히 하기 위해 다른 사람들의 정리글을 보면서 시도했는데, 위 글이 가장 깔끔하게 잘 작성되어있는 듯

2번 글에서, 레포지토리를 클론하는 대신, 직접 로컬로 받아서 해당 레포에 넣어주면 된다

## 6. 세부 문제 수정
---

로컬 구동까지 확인한 뒤, 원격 저장소에 올리니 에러가 발생했다

![Image](https://github.com/user-attachments/assets/d63d63c7-860b-453d-9a94-545c5de9b21a)

`Error: Can't find stylesheet to import.`

해당 스타일시트를 찾을 수 없다고 뜨는 것 같은데, 

.gitignore에 `_sass/vendors`와 `assets/js/dist`가 들어있어 커밋이 되지 않아서

gitAction에서 빌드를 할 수 없어 발생하는 오류였다

`git add assets/js/dist/ _sass/vendors/ -f`로 예외 처리된 내용을 직접 푸시하여 

빌드,배포를 성공 할 수 있었다

## 7. 얻은 것
---

이번 고생하면서 CI/CD 과정과, GitAction, Jekyll, Ruby 등에 대해 학습할 수 있었다

한 달 간의 삽질 끝에 결국 블로그를 정상적으로 운영할 수 있게 되었다

![Image](https://github.com/user-attachments/assets/f3f9cd41-813d-46d0-ae9a-0f709771a88b)

이 맑은 눈의 광기 개미 볼때마다 빡친다

티스토리나 Velog로 갈아탈까 몇번씩이나 고민한 순간도 많았지만, 해결하고 나니 뿌듯하다

다음 에러 터지기 전까진 계속 쓸 예정이다

끝