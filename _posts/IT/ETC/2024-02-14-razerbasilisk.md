---
title: "레이저 바실리스크 얼티메이트 휠튐증상 수리"
# excerpt : 요약
date: 2024-02-14 10:41:00 +0900
# last_modified_at: 
categories: [IT, ETC] # 최대 2개 가능
tags: [razer, basilisk ultimate, repair, mouse]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://img.danawa.com/prod_img/500000/599/129/img/10129599_1.jpg?_v=20221129161707"
    alt: 
---

레이저 바실리스크 얼티메이트 마우스를 사용한지 2년쯤 되니 

레이저의 고질병인 마우스 휠튐증상이 나타났다

11번가에서 직구해서 AS도 사설에 맡겨야하길래 직접 수리하기로 결정했다

![휠튐증상](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f2ce980f-2f93-41df-b33b-17b516f3f7d7)

위로 올리는데 아래로 튀는 증상이 발생함

몇번 분해해서 고양이털좀 빼주면 멀쩡해졌는데

이제는 청소해도 나아지지가 않아서 휠 인코더를 교체하기로 결정

# 마우스 휠 인코더란?
---

![scroll-wheel-mouse-sealed-rotary-encoder13433030751](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f07a90a8-a7a1-4fae-8c10-ea9374946be7)

휠 축은 한쪽 면이 각이져있는데, 이 면이 스크롤 휠 인코더 6각형에 결합되었을 때,

휠이 회전하게되면 같은 방향으로 인코더의 디스크 바퀴를 회전시키게된다.

스크롤 휠 인코더의 내부에는 발광부와 수광부가 있는데,

휠 인코더의 디스크 바퀴가 회전할 때 마다 2개의 구형파를 발생시킨다

2개의 구형파는 DSP의 1, 2번핀에 입력되고, DSP는 입력된 두 구형파의 위상을 비교하여 

위로 이동하는 동작인지, 아래로 이동하는 동작인지를 판단한다.

또한, DSP는 입력되는 구형파의 개수를 카운트하여 이동량과 속도를 산출한다

아래 동영상에 마우스 휠 원리가 자세히 설명되어있음

<iframe width="560" height="315" src="https://www.youtube.com/embed/-HVKm5fIUA8?si=-ZDMvpy0GTb9U6FF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# 레이저 바실리스크 얼티메이트 수리
---

### 수리에 필요한 물품들

1. 교체할 휠 인코더. 레이저 맘바용 인코더 호환 가능
    - [레이저 맘바용 인코더](https://ko.aliexpress.com/item/1005002258383846.html?spm=a2g0o.order_list.order_list_main.27.3b5a140fdBZ50b&gatewayAdapt=glo2kor)
2. T6 별나사 드라이버 + PH0 소형 드라이버
3. 교체할 마우스 피트 (기존 피트 재활용 가능)
    - [바실리스크 얼티메이트용 마우스 피트](https://ko.aliexpress.com/item/1005001969050396.html?spm=a2g0o.order_list.order_list_main.71.3b5a140fdBZ50b&gatewayAdapt=glo2kor)

# 과정

![피트 및 분해](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/27134341-bb54-4bd8-8fc3-556d174ea0db)

피트를 벗기고, 아래 별나사 5개를 T6 별나사 드라이버를 이용해 풀어준다

![휠 분해](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/52f25913-4527-48af-a123-c71e786ca900)

휠 인코더 단자를 빼고, 휠을 소켓에서 꺼낸다 

![휠 분해](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/142ebfa4-7a84-46e4-b392-0993cae332c7)

휠 위쪽을 잡고 아래쪽 플라스틱 고정부를 내리면 빠짐


![화면 캡처 2024-02-14 112337](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/8b9b8ef7-d3a5-4633-a3a1-83dcf2a3603e)

이후, 휠 오른쪽의 RGB 스크롤 모듈 옆의 나사를 빼준다

![휠 고정부 분해](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/43679062-0822-4c79-9aa8-6b5ee8538e5f)

RGB 스크롤 모듈에서 나사를 풀면, 휠 모듈이 아예 빠지는데

여기서 휠을 고정해주는 로드를 제거

나사를 뺀 후, 휠을 위로 올리면 빠짐. 단단히 고정되어있으니 약간의 힘 필요

빠지면서 왼쪽의 휠 인코더도 같이 빠짐

![IMG_3266](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/f8b45ced-1c6d-4f20-8370-68efb4f2306b)

왼쪽이 기존의 얼티메이트용 휠 인코더

오른쪽 노란색이 새로 구매한 맘바용 휠 인코더. 호환 가능하다

휠에서 기존 인코더를 빼주고 새 인코더를 껴주고

결합은 분해의 역순으로 진행해주면 끝


![수리 완료](https://github.com/jinhg0214/jinhg0214.github.io/assets/70011316/7bd4648e-b1da-4f75-8583-657d98041f3b)

잘된다!


참조 :

[광마우스의 구조와 동작 원리](https://m.blog.naver.com/kangyh5/222192262224)

[레이저 바실리스크 얼티메이트 휠 인코더 교체 ](https://m.blog.naver.com/aujin0998/223104193498)

[레이저 분해 영상](https://www.youtube.com/watch?v=AR9x18ydEYU)