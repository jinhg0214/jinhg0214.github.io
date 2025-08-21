---
title: "Git 사용 시 'kex_exchange_identification' 오류 해결 방법"
date: 2025-07-28 10:00:00 +0900
categories: [IT, Utility]
tags: [git, error, token, pat]
toc: true
comment: true
published: true
image:
    path: ""
    alt: ""
---

## 증상

라즈베리파이에서 Git Clone을 해오려니 에러가 발생했다.

찾아보니 2021년 8월 13일부터 GitHub에서는 보안상의 이유로 

비밀번호를 이용한 Git 인증을 더 이상 지원하지 않는다고 한다

대신 Personal Access Token(PAT)을 발급받아 사용해야 한다고함

이전에 비밀번호로 잘 사용하던 환경에서 갑자기 이 오류가 발생했다면, 이 정책 변경 때문일 가능성이 높음


## 해결 방법

GitHub에서 Personal Access Token을 발급받아 비밀번호 대신 사용하면 끝임

### 1. Personal Access Token (PAT) 발급받기

1.  GitHub 웹사이트에 로그인한 후, 우측 상단의 **프로필 아이콘**을 클릭하고 **Settings**로 이동
      ![token](https://miro.medium.com/v2/resize:fit:1400/1*lDfi0wXa-Q0aW0j5gQ7W5g.png)

2.  왼쪽 메뉴에서 **Developer settings**를 클릭

3.  **Personal access tokens** > **Tokens (classic)**을 선택

4.  **Generate new token** 버튼을 클릭하고, **Generate new token (classic)**을 선택

5.  **Note** 필드에 토큰의 용도를 알아보기 쉽게 적기 (예: `raspberrypi-git-access`)

6.  **Expiration**을 설정. 보안을 위해 만료 기간을 설정하는 것을 권장함. (예: 30일)

7.  **Select scopes**에서 이 토큰에 부여할 권한을 선택. `repo` 스코프는 private 저장소에 대한 접근을 포함하여 대부분의 작업을 허용.

8.  **Generate token** 버튼을 클릭

9.  생성된 토큰은 **다시 볼 수 없으므로** 안전한 곳에 즉시 복사해두어야한다. 토큰은 `ghp_`로 시작하는 문자열

### 2. 발급받은 토큰으로 Git 명령어 실행하기

이제 `git clone`이나 `git pull`을 실행할 때, 비밀번호를 묻는 프롬프트가 나타나면 복사해둔 Personal Access Token을 붙여넣기 합니다.

```bash
$ git clone https://github.com/your_username/your_repository.git
Username for 'https://github.com': your_username
Password for 'https://your_username@github.com': # 여기에 발급받은 PAT를 붙여넣기
```

패스워드는 투명처리되어 보이지 않으므로 주의

이렇게 하면 정상적으로 Git 명령어가 실행됩니다.

**주의사항:**
*   공용 PC나 신뢰할 수 없는 환경에서는 토큰의 만료 기간을 짧게 설정하는 것이 안전함
*   토큰은 비밀번호와 같으므로 절대 외부에 노출되지 않도록 주의!!!!