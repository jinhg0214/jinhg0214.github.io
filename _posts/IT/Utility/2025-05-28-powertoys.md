---
title: "윈도우 PowerToys 4가지 유용 유틸 소개"
date: 2025-05-28 22:24:47 +0900
categories: [IT, Utility]  # 최대 2개 가능
tags: [windows, tools, ocr, design]     
toc: true
comment: false
published: true
image:
    path: "https://images-eds-ssl.xboxlive.com/image?url=4rt9.lXDC4H_93laV1_eHM0OYfiFeMI2p9MWie0CvL99U4GA1gf6_kayTt_kBblFwHwo8BW8JXlqfnYxKPmmBRY0umJKt.GQbXbqRe3O0.PRqyi1SW2G7RHi4dtRbVj6pfgLfV_GTUPIMpwhbsrNfF_oVsq.XJT1.6ZGkeTPoqI-&format=source)"
    alt: 
---

## 윈도우 파워토이즈란

PowerToys는 마이크로소프트에서 개발한 무료 유틸리티 모음으로, 

윈도우 사용자의 생산성을 크게 향상시켜주는 도구들의 모음이다

오늘은 그 중에서도 특히 유용한 4가지 기능에 대해서 설명해보겠음

## 1. Always On Top - 창을 항상 맨 앞에

![aot](https://learn.microsoft.com/ko-kr/windows/images/pt-always-on-top.png)

특정 창을 다른 모든 창보다 앞에 고정시켜주는 기능

작업하면서 참고해야 할 문서나 도구를 항상 보이게 하고 싶을 때 매우 유용함

1. 맨 앞에 고정하고 싶은 창을 선택
2. ``Win + Ctrl + T`` 
3. 창의 테두리에 파란색 테두리가 생기면 Always on Top 적용 된 것
4. 해제하려면 동일한 단축키를 다시 입력

### 활용 예시

- 영상 시청 중 메모장을 항상 앞에 두고 내용 기록
- 개발 작업 시 참고 문서를 항상 보이게 유지
- 온라인 강의 수강 중 필기 프로그램을 맨 앞에 고정

## 2. Color Picker - 화면의 모든 색상 추출

![cp](https://learn.microsoft.com/ko-kr/windows/images/pt-colorpicker-hex-editor.png)

화면의 어떤 부분이든 마우스로 클릭하여 해당 위치의 색상값을 추출할 수 있는 도구

디자인 작업이나 웹 개발 시 매우 유용함

1. `Win + Shift + C` 단축키로 Color Picker 활성화
2. 마우스 커서를 색상을 추출하고 싶은 위치로 이동
3. 클릭하면 색상값이 클립보드에 자동 복사
4. 다양한 색상 형식으로 결과 확인 가능 (HEX, RGB, HSL 등)

### 지원 색상 형식

- **HEX**: #FF5733
- **RGB**: rgb(255, 87, 51)
- **HSL**: hsl(9, 100%, 60%)
- **HSV**: hsv(9, 80%, 100%)
- 그 외에도 다양한 설정 가능

### 활용 예시

- 웹사이트의 특정 색상을 따와서 디자인에 적용
- 이미지에서 마음에 드는 색상 추출
- 브랜드 가이드라인 작성 시 색상 코드 수집

## 3. 마우스 찾기 (Find My Mouse) - 마우스 커서 위치 파악

![mouse](https://learn.microsoft.com/ko-kr/windows/images/pt-mouse-utilities-find-my-mouse.gif)

듀얼 모니터나 대형 화면에서 마우스 커서를 찾기 어려울 때 유용한 기능

단축키를 누르면 마우스 커서 주변에 원형 하이라이트가 나타나 쉽게 찾을 수 있다

1. `Ctrl` 키를 두 번 빠르게 누르기
2. 마우스 커서 주변에 점점 커지는 원형 하이라이트 표시
3. 애니메이션 효과로 마우스 위치를 명확하게 인식

### 설정 옵션

- **활성화 방법**: Ctrl 더블 클릭 또는 흔들기 감지
- **하이라이트 색상**: 사용자 정의 가능
- **애니메이션 지속 시간**: 조절 가능
- **스포트라이트 반지름**: 크기 조정 가능

### 활용 예시

- 멀티 모니터 환경에서 마우스 커서 분실 시
- 프레젠테이션 중 청중에게 마우스 위치 표시
- 대형 화면에서 작업할 때

## 4. 텍스트 추출기 (Text Extractor) - 이미지에서 텍스트 추출

![te](https://cdn.mos.cms.futurecdn.net/fbA5pE5bpzgQ5CYu773eB9.gif)

화면의 이미지나 PDF에서 텍스트를 인식하여 복사 가능한 텍스트로 변환해주는 

OCR(Optical Character Recognition) 기능

생성된 텍스트의 정확도가 떨어질 수 있으므로 추후 교정이 필요하지만 쓸만함

OCR 언어 팩이 설치된 언어만 인식할 수 있음(한국어 O)

### 사용 방법

1. `Win + Shift + T` 단축키로 텍스트 추출 모드 활성화
2. 마우스로 텍스트가 포함된 영역을 드래그하여 선택
3. 자동으로 텍스트 인식 후 클립보드에 복사
4. 원하는 곳에 붙여넣기로 사용

### 지원 기능

- **다국어 지원**: 한국어, 영어, 일본어, 중국어 등
- **정확한 인식**: 고품질 OCR 엔진 사용
- **즉시 복사**: 인식된 텍스트가 자동으로 클립보드에 저장
- **미리보기**: 인식 결과를 확인 후 복사 가능

### 활용 예시

- PDF 문서에서 복사가 안 되는 텍스트 추출
- 스크린샷이나 이미지 파일의 텍스트 내용 복사
- 웹페이지의 보호된 텍스트 추출
- 책이나 문서를 촬영한 사진에서 텍스트 변환

물론 저작권이 허가하는 범위 내에서 사용해야하므로 주의

## PowerToys 설치 및 설정

### 설치 방법

1. **Microsoft Store**에서 "PowerToys" 검색 후 설치
2. **GitHub 릴리즈 페이지**에서 직접 다운로드
3. **winget** 명령어 사용: `winget install Microsoft.PowerToys`

등 편한 방법으로 설치하면 된다

처음 설치 시 굉장히 많은 기능들이 추가되므로, 자기한테 필요한 기능을 체크하고 모두 끄는 것을 추천함

## 마무리

PowerToys의 이 4가지 기능만으로도 일상적인 컴퓨터 작업의 효율성을 크게 향상시킬 수 있다

각 기능들은 단독으로 사용해도 유용하지만, 조합하여 사용하면 더욱 강력한 생산성 도구가 된다

특히 개발자, 디자이너, 그리고 다양한 문서 작업을 하는 사용자들에게는 

없어서는 안 될 필수 도구라고 할 수 있다고 생각함

---

**관련 링크**:

- [PowerToys 공식 GitHub](https://github.com/microsoft/PowerToys)
- [Microsoft Store - PowerToys](https://learn.microsoft.com/ko-kr/windows/powertoys/)