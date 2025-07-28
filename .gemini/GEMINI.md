- 이 프로젝트는 Jekyll 기반의 GitHub Pages 블로그입니다.
- 기술 스택은 Jekyll을 사용하며, Chirpy 테마를 적용했습니다.
- `_posts` 디렉토리에는 직접 작성한 포스팅들이 있습니다.
- 몇 가지 버그가 있지만, 동작은 하고 있습니다.
post의 기본 포멧은 다음과 같습니다

```
---
title: 
date: YYYY-MM-DD HH:MM:SS +0900
categories: [메인 카테고리, 서브 카테고리]  
tags: [tag1, tag2, tag3](추가시 5개까지만 추가)     
toc: true
comment: false
published: true
image:
    path: (관련된 이미지)
    alt: 
---

(글 시작)

```

깃 컨벤션은 다음과 같습니다 https://github.com/conventional-changelog/commitlint/#what-is-commitlint