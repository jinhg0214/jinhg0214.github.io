---
title: "파이썬 : 파이참 입출력 파일 설정"
date: 2025-05-16 17:08:00 +0900
categories: [Python, Util]  # 최대 2개 가능
tags: [ide, pycharm, input, output, setting]
toc: true
comment: false
published: true
image:
    path: "https://www.jetbrains.com/pycharm/download/img/download.png"
    alt: 
---

Visual Studio의 `freopen_s(new FILE*, "input.txt", "r", stdin)`처럼

파이참에서 파일을 읽어 이를 결과로 기록하는 방법은 없나 찾아보다가 세팅이 있어서 기록해둠

![image](assets\img\posts\20250516_1.png)

파이참 실행 버튼 아래 구성 편집 클릭

![image](assets\img\posts\20250516_2.png)

새 구성 추가 > Python 누르면 우측처럼 새 창이 뜨는데

![image](assets\img\posts\20250516_3.png)

옵션 수정을 누르면 숨겨진 옵션들이 뜨는데 

여기서 `운영 체제 > 입력 리디렉션`, `로그 > 콘솔 출력을 파일에 저장` 을 활성화 한다

이후 두번쨰 그림에 빨간색으로 표시된 칸들을 각각 

script : 자신의 `main.py` 파일

다음 위치의 입력 리디렉션 : 입력으로 사용할 `input.in` 파일

콘솔 출력을 파일에 저장 : 출력으로 저장할 `output.out` 파일

경로를 지정해주면 된다

여기서는 `main.py`의 같은 폴더 내에 input, output 파일을 만들어줬음

![image](assets\img\posts\20250516_4.png)

이후 입력을 테스트해보면 

input.in 파일의 내용이 output.out 파일에 정삭적으로 기록된 것을 확인할 수 있음