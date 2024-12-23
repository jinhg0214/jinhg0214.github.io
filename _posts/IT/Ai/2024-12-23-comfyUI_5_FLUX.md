---
title: "ComfyUI 5. FLUX"
date: 2024-12-23 21:30:00 +0900
categories: [IT, Ai]  # 최대 2개 가능
tags: [image, generator, comfyui, flux, text-to-image, blackforestlabs]   
toc: true
comment: false
published: true
image:
    path: "https://miro.medium.com/v2/resize:fit:1400/1*-opEtjv6dtRg7IW6S0U_Sg.png"
    alt: 
---

[ComfyUI 4. ComfyUI Manager & ControlNet](https://jinhg0214.github.io/posts/comfyUI_4_controlnet/)에 이은 FLUX를 comfyUI에서 사용하는 법

## FLUX란?
---

Black Forest Labs(Stable Diffusion 만든곳)에서 개발한 모델

사실적이고 자연스러운 이미지 생성과 무려 8K까지 고해상도 출력을 지원하는 모델이다

2024년 12월 기준 FLUX1.1 Pro, FLUX.1 Pro, FLUX1. Dev, FLUX1. Schnell 총 네 종류가 있음

![pro](https://blackforestlabs.ai/wp-content/uploads/2024/11/1152-716993-2048x1170.png)

![pro2](https://blackforestlabs.ai/wp-content/uploads/2024/11/1142-1683782705-2048x1170.png)

Pro 버전은 최대 8K까지 가장 높은 수준의 이미지 생성이 가능하고, 정교한 세부 표현이 가능하다

전문적 사용을 위해 설계 되었으며, 별도의 라이선스 구매가 필요하다

API를 이용하여 서버에 직접 요청하거나, 파트너 사이트에서만 출력 가능함

Dev 버전은 Pro버전보다 경량화 되어 더 빠르고 효율적이다

연구 및 개발 목적에 적합하고, 비상업적 용도로만 사용이 가능한 라이선스가 적용된다

Schnell 버전은 속도에 최적화된 버전, 로컬개발 환경과 개인 사용에 적합하다

독일어로 빠른이라는 뜻이라고 함

슈넬치킨이 빠른 치킨이란 뜻이였구나


## 설치법
---

Dev, Schnell 버전을 허깅페이스에서 다운로드한다

[FLUX.1-dev 레포지토리](https://huggingface.co/black-forest-labs/FLUX.1-dev)

[FLUX.1-schnell 레포지토리](https://huggingface.co/black-forest-labs/FLUX.1-schnell)

### 1. 체크포인트 모델 다운
flux1-dev.safetensors 파일과 flux1-schnell.safetensors 파일을 받아 

`ComfyUI\models\unet`폴더에 넣어준다

각 모델이 23.8GB 이므로 용량에 주의할것

### 2. VAE

마찬가지로 해당 레포에서 VAE 폴더 내의 

`diffusion_pytorch_model.safetensors` VAE 파일을 받아 `ComfyUI\models\vae` 폴더에 넣는다

두 버전 모두 같은 VAE이므로 둘 중 하나만 받아 넣으면 된다

### 3. 텍스트 인코더(생략 가능)

마찬가지로 해당 레포에서 필요하다면 text encoder를 받아 폴더에 넣기

### 4. t5xxl_fp16.safetensors

[해당 링크](https://huggingface.co/comfyanonymous/flux_text_encoders/tree/main)에서 

`t5xxl_fp16.safetensors` 혹은 ` clip_l.safetensors`를 받아 

`ComfyUI/models/clip/` 폴더에 넣는다

## 예제 실행해보기
---

comfyUI에서 만든 튜토리얼이 있다

https://comfyanonymous.github.io/ComfyUI_examples/flux/?ref=blog.comfy.org

### dev버전 테스트

위 링크의 Flux Dev 문단의 이미지를 끌어다 comfyUI에 놓으면 워크플로우를 불러올 수 있다

![Honeycam 2024-12-23 21-55-19](https://github.com/user-attachments/assets/2d57850a-4bca-467a-b175-6b139d31ac93)

불러온 워크플로우를 실행했을 때 일부 노드가 빨간색으로 표시되는 경우, 

ComfyUI 매니저를 통해서 각 노드를 업데이트 해보고

체크포인트, vue 모델, encoder 경로를 다시  재설정해주면 된다

![image](https://github.com/user-attachments/assets/66e9e8b2-9cf9-44d6-b426-20ccea6811d7)
*출력 결과. 손같은 디테일이 확실히 엄청나다*

![image](https://github.com/user-attachments/assets/a86668ec-9d23-4208-bc77-8ed7fc8a8a1b)
*경로 설정*

dev는 VRAM 사용량이 많아 `fp8_e4m3fn_fast`로 변경해주었음

텍스트 인코더도 `t5xxl_fp16`에서 `t5xxl_fp8`로 변경

### Schnell 테스트

마찬가지로 Flux Schnell 문단의 사진을 comfyUI로 끌어다 놓으면 워크플로우가 생성된다

dev와 마찬가지로 model, encoder, vae 경로를 잘 설정해주면 생성된다

dev 버전과의 차이점은 `FLUXGUIDANCE`노드가 없다는 점이다

![image](https://github.com/user-attachments/assets/cfb2ff94-0a69-45ba-9aa5-701f46a29602)
*잘 출력된 모습*

![image](https://github.com/user-attachments/assets/98e2c8ab-1159-4806-b64d-a34a82846952)
*dev 와 같은 프롬프트를 출력한 모습. 확실히 손 같은 디테일이 차이가 난다*