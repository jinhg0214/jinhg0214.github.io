---
title: "토탈워 워해머3 모딩"
# excerpt : 요약
date: 2024-11-12 19:03:00 +0900
# last_modified_at: 
categories: [Game, totarwar] # 최대 2개 가능
tags: [warhammer, modding, database]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://static1.dualshockersimages.com/wordpress/wp-content/uploads/2022/10/10-essential-mods-for-warhammer-3-immortal-empires.jpg"
    alt: 
---

![1731394252](https://github.com/user-attachments/assets/5979fe57-dee0-4abf-a76d-825c74cc8063)

[햄탈워의 속도, 돌격속도, 가속/감속 및 회전속도에 대해서 뻘글](https://gall.dcinside.com/mgallery/board/view/?id=ttwar&no=1909881&exception_mode=recommend&page=1)

위 글을 보고, 토탈워 모딩을 직접 해보았다

[Total War Modding](https://tw-modding.com/wiki/Main_Page)

## 1. 준비물
---
- [Rpfm](https://github.com/Frodo45127/rpfm) : 토탈워 시리즈 모딩툴
- [간단한 Rpfm 사용법 및 모딩 튜토리얼 선행학습](https://www.youtube.com/watch?si=_1a0c3WUJbGxLl_d&v=IXFdaSJ6q78&feature=youtu.be)


## 2. 목표
---

스카브란드한테 해당 속성 달아주기

## 3. 과정
---
토탈워의 모딩은 테이블에서 값을 복사해서, 수정한 뒤 이를 덮어씌우는 방식으로 동작하는듯 하다

크게 다음 과정을 따름
```
1. 게임 data.pack 의 db 폴더 내에서 수정하고자 하는 테이블을 찾고
2. 새로 만든 모딩.pack 파일에서 같은 이름의 테이블을 생성한 뒤에
3. 새로울 열을 추가한 뒤 값을 수정하고 저장, 적용한다
```

먼저 내가 수정할 유닛의 이름이 뭔지를 알아야한다

`land_units_tables`을 열면

![table](https://github.com/user-attachments/assets/d66c0b4e-51dd-47c2-a4d0-293fd3b5ade1)

무수히 많은 정보들이 반겨주는데

하단의 검색창에서 원하는 캐릭 이름 검색

![search](https://github.com/user-attachments/assets/319b30bc-9c00-472c-af47-3ef6f4bd2fd0)

1292열에 스카브란드의 내용이 있다

스크롤을 옆으로 움직이며 확인해보면

여러 내용들이 있는데, 체력, 근공, 근방같은 간단한 값들은 여기서 수정 가능하다

![attr](https://github.com/user-attachments/assets/dae71127-0d56-44a6-ba69-2d38c0544f7b)

여기서 스카브란드의 `Man Entity` 값을 보면 `wh3_main_kho_skarbrand_blood` 라는 값을 가지는데

이를 잘 기억해 둔다

![battle_entity](https://github.com/user-attachments/assets/df5ed65e-5e51-4812-9a4d-b39fb3d66299)

토탈워 전투에서 이동속도 관련된 내용은 `battle_entities_tables` 내에 저장되어있다

이 테이블을 열면 캐릭별로 속성들이 저장되어있는데 위에서 기억해둔 스카브란드의 값을 찾는다

![skarbrand](https://github.com/user-attachments/assets/dd89b078-e93b-4e12-8a8e-cde63d252aad)

733번 줄에 스카브란드의 이동속도, 달리기 속도, 가속, 감속 등의 정보가 있음

이 값을 오른쪽 끝까지 쭉 복사해서 내 테이블에 추가한다

![my_table](https://github.com/user-attachments/assets/d6653227-5c4b-41fc-82b3-9bdd75eb89d6)

잘 가져와진 모습

이후 이 값들을 원하는대로 수정한 뒤, 저장한다

`Walk Speed, Run Speed, Accelation, Decelation, Charge Speed, Charge Distance Pick Target, Turn Speed` 등

위 짤과 관련되어 있어보이는 값들을 적당히 수정했다

![modmanager](https://github.com/user-attachments/assets/1bc4beb2-b1cc-400b-8d30-3f5c37630a72)

PackFile > Install 하면 data폴더 내에 자동으로 pack 파일이 들어가고

모드매니저에서 활성화 해주면 끝

![result](https://github.com/user-attachments/assets/ae15459b-034b-4d5b-995b-2a20d966493b)

이속, 돌격, 질량등을 수정해서 시원하게 밀고 다닌다

---

직접 특성을 추가해서 할당해주는 방식으로 해주고 싶었는데

커스텀 특성을 추가하는 방법을 몰라서 그냥 하드코딩으로 값을 수정해버림

추후에 더 수정하게 된다면, 스킬을 작성하고, 이를 영웅에 할당하는 식으로 쓸듯