---
title: "Github Pages 공개 범위 설정"
# excerpt : 요약
date: 2023-10-17
# last_modified_at: 
categories: [IT, GitPage] # 최대 2개 가능
tags: [web, scope]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
---

![key](https://images.unsplash.com/photo-1585914641050-fa9883c4e21c?auto=format&fit=crop&q=80&w=1467&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D){: .width="350px"}

# 1. Private가 필요한 이유?
---
깃 페이지를 기술 블로그가 아닌 일상 사진들도 저장하는 용도로 사용하고 싶음

- 기술 정리는 옵시디언을 이용하고 있음

- 일단 옵시디언에 정리해두고, 중요한거나 공유하고 싶은 내용은 다듬어서 깃페이지에 저장할 예정

- 그 외에 개인적인 사진들도 깃 페이지에 저장해두고 아카이브로 쓰고싶음 
  
- post 설정을 `published : false`로 지정할수도 있겠지만,    
  깃 페이지 소스코드에서 노출됨(누가 들어오겠냐마는).    

- GitPage는 Pro 계정이 아니라면, 공개 범위가 public으로 고정되어있음.   

이러한 문제들 때문에 private의 필요성을 느끼게 되었음.   

# 2. 현재 생각난 해결법들
---
1. Git Pro 사용
    - 월 사딸라
    - 아직까진 그정도까지 필요성을 못느끼고 있음
2. private 레포를 판 다음 여기에 소스코드를 붙여넣는다
    - 더이상 Minimal Mistakes의 업데이트를 받지 못함
3. 다른 블로그로 전환?
    - 현재 유력 후보는 티스토리 
    - 깃페이지 열심히 파놓고 또 옮기는건 좀 아쉽긴 함

더 좋은 방법이 있는지 찾아봐야겠다