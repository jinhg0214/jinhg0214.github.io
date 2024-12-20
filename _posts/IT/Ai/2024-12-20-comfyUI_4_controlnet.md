---
title: "ComfyUI 4. ComfyUI Manager & ControlNet"
date: 2024-12-18 13:00:00 +0900
categories: [IT, Ai]  # 최대 2개 가능
tags: [image, generator, image-to-image, comfyui, controlnet]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://github.com/Fannovel16/comfyui_controlnet_aux/raw/main/examples/CNAuxBanner.jpg"
    alt: 
---

[ComfyUI 3. Image-to-image]https://jinhg0214.github.io/posts/comfyUI_3/)에 이어서 컨트롤넷을 사용하는 방법에 대해 알아보기

## ComfyUI Manager
---

![cm](https://github.com/ltdrdata/ComfyUI-Manager/raw/main/misc/menu.jpg)

ComfyUI 매니저는 ComfyUI 플러그인들을 쉽게 관리하고 설치할 수 있게 해주는 확장프로그램이다

다른사람의 워크플로우를 파일로 직접 가져오거나, 커스텀 노드를 직접 Git Bash로 설치할 필요없이

ComfyUI 매니저를 통해서 모든 커스텀 노드들을 다운 받고 관리할 수 있다

[ComfyUI-Manager](https://github.com/ltdrdata/ComfyUI-Manager)

### 설치

comfyUI를 설치하면서 git이 깔려있다는 가정하에

1. `ComfyUI/custom_nodes` 경로로 이동 후, cmd를 관리자권한으로 실행
2. `git clone https://github.com/ltdrdata/ComfyUI-Manager.git`
3. ComfyUI 재시작

![image](https://github.com/user-attachments/assets/e7fbb277-ce0a-4f57-9f03-f208b9599522)

우측 상단에 Manager 버튼이 생성되었고 정상적으로 눌린다면 설치 완료

### 기능

![cm](https://github.com/ltdrdata/ComfyUI-Manager/raw/main/misc/menu.jpg)

1. Custom Nodes Manager : 커스텀 노드 검색 및 설치. 원버튼으로 알아서 설치해줌
2. Install Missing Custom Node : 현재 워크플로우에 없는 커스텀 노드 설치
3. Model Manger : 체크포인트 모델 검색. 근데 CivitAI에서 직접 받는걸 추천함
4. Update All : 커스텀 노드 모두 업데이트
5. Restart

## ControlNet
---

![cn](https://miro.medium.com/v2/resize:fit:1400/1*XUb9XWj5DuBiZNBsoW3l0g.png)

기존 이미지를 해석하여 해당 이미지의 구도, 포즈, 깊이감 등을 따라 만들 수 있는 기능

컨트롤넷 이전에는 생성 인공지능은 랜덤성 때문에 창작자들의 실무에 활용하기 어려웠다고 한다

랜덤한 결과물들만 출력 가능하고, 창작자 의도대로 제어가 불가능했기 때문

![raemn](https://github.com/user-attachments/assets/5a139145-012e-4455-9ad0-a9235b7f8e1a)

*대충 이런느낌*

그러나 컨트롤넷 모델을 사용하여 이러한 무작위성을 제어할 수 있게되어, 

더 효과적으로 원하는 이미지 생성을 할 수 있게 되었다고 한다

## 주요 기능

![controlNet](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F5bb0de50-a0fb-42ae-b4b8-cf009e907f56%2F292bbc6b-7085-462d-9f4a-a7286dd70a61%2FGroup_1.png&blockId=feb43781-4103-4d04-9efb-85bad29b11a1)

1. 일러스트의 캐릭터에서 포즈를 따오는 Openpose
2. 캐릭터나 사물의 윤곽선을 따오는 Canny
3. 사물의 깊이를 따오는 Depth 
4. 사물의 형태를 선으로 표현한 Lineart
5. 그 외에도 Tile, Softedge 등

### 설치

comfyUI 매니저를 받았다면 `Manger > Custom Nodes Manger`에서 

`ComfyUI's ControlNet Auxiliary Preprocessors`를 검색해서 설치한다

혹은 직접 [깃허브](https://github.com/Fannovel16/comfyui_controlnet_aux)로 이동해서 설치법 보고 설치

### 노드 배치 

![image](https://github.com/user-attachments/assets/bff9de60-f0c2-4f92-84d3-f12a6da2c241)

T2T를 베이스로 컨트롤넷 파트를 추가해준다

`Load ControlNet Model` : 사용하고자 하는 컨트롤넷 모델  
CivitAI나 Hugging Face에서 controlNet으로 검색해서 해당 controlNet 폴더에 넣어줘야한다   
자기가 사용하고자 하는 체크포인트 모델에 맞는 컨트롤넷을 사용해야 제대로 인식한다

`Load Image` : 불러올 이미지 선택하는 노드

`AIO Aux Preprocessor` : 컨트롤넷에서 이미지 분석을 위한 전처리 과정에 사용하는 노드   
preprocessor 파라미터에서 원하는 기능을 선택한 뒤 분석하면 된다

![image](https://github.com/user-attachments/assets/e501c3b3-1fa5-422b-ba3b-e52ebd5966c9)
*순서대로 자세, 윤곽선, 깊이, 테두리 분석*

`Apply ControlNet` : 전처리된 이미지를 KSampler에게 전달하는 노드   
분석한 출력을 KSampler의 긍정, 부정 프롬프트로 넣어주면 된다   
strength 파라미터로 얼마나 해당 전처리를 따를지 정할 수 있다

![image](https://github.com/user-attachments/assets/96863344-1d49-4c0b-bb56-7741faca8d6d)
*실제 출력 결과*

잘 된 다!