---
title: "매크로 탐지를 회피하는 user-agent"
date: 2024-02-02 15:44:00 +0900
categories: [IT, Web]  # 최대 2개 가능
tags: [scrapping, crawler, crawling, python, beautifulsoup, requests, selenium, user-agent]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true

image:
    path: "https://images.marketpath.com/9dda7041-2870-4a96-bf1e-dd8342e86e7c/p/692f7bc6-6bfd-4f4c-933b-54fd8e88461b/1920wx1080h-max/user-agents-blog-post.png"
    alt: 

---

웹 스크래핑을 하는 도중, 특정 사이트에서 503 에러가 발생했다

전체 게시물을 긁어오는건 문제없이 200을 뱉었으나,

특정 키워드를 넣어 검색시 503 에러가 발생하였다

robot.txt가 정의되어있지 않아 어떤 이유인지 정확히 파악이 되지않아 이것저것 검색해본 결과

`user-agent`라는 식별번호를 request header에 넣어 보내는 방법을 알게 되었다

# 1. user-agent란?
---

selenium이나 beautifulsoup 를 사용 시, 서버측에서 매크로로 판단하고 접속을 끊어버리는 경우가 발생한다

이를 해결하기 위해, 자신의 정체를 알리는 `User-Agent`라는 HTTP 헤더를 보냄

일종의 신분확인증같은 개념인듯함

이 문자열은 보통 브라우저의 종류, 버전 번호, 호스트 운영체제 등의 정보를 포함한다

웹 초기에는 브라우저이름, 버전만 담을 정도로 간단했으나

웹이 점점 발달하면서 다양한 정보들을 포함하게 되었다고 한다


스팸 봇, 다운로드 관리자, 일부 브라우저는 자신의 정체를 숨기고 다른 클라이언트인 척 하기위해

가짜 user-agent를 보내기도 하는데, 이를 user-agent spoofing이라고 함


# 2. 어떻게 매크로 탐지를 우회하는가?
---

user-agent를 정의하지 않고 웹 크롤링을 하면, 

일반적으로 매크로는 실제 브라우저와는 다른 user-agent를 사용하기 때문에 여기서 걸린다고 함

아니면 이러한 정보가 없는 사용자는 매크로로 온 것으로 판단하고 접근을 차단한다고 한다

혹은, 웹 사이트는 특정 브라우저 버전 또는 운영체제에 맞춰 설계되기 때문에, 

이런 정보들이 주어지지 않으면 페이지를 제대로 표시할 수 없어 차단하기도 한다고 함

이러한 이유들 때문에 user-agent가 없거나 실제 정보와 일치하지 않으면 차단하기 때문에

user-agent가 정의되어있으면 대부분 통과 된다고 한다

# 3. 사용 방법
---

[whatismybrowser.com](https://www.whatismybrowser.com/detect/what-is-my-user-agent/)

자신의 user-agent는 위 사이트에서 확인할 수 있다

이를 복사해서


### Selenium

```python
from selenium import webdriver

options = webdriver.ChromeOptions()
user_agent = "위에서 복사한 내 user-agent"
options.add_argument("--user-agent=" + user_agent)

driver = webdriver.Chrome(options=options)
```

### beautifulsoup

```python
import requests
from bs4 import BeautifulSoup

headers = {
    "User-Agent": "위에서 복사한 내 user-agent"
}

response = requests.get("https://www.naver.com", headers=headers) 
soup = BeautifulSoup(response.text, "html.parser")
```

처럼 넘겨주면 매크로 차단을 피할 수 있다

