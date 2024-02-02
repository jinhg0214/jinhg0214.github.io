---
title: "웹 크롤링과 웹 스크래핑, Beautiful Soup와 Selenium"
date: 2024-02-01 18:00:00 +0900
categories: [IT, Web]  # 최대 2개 가능
tags: [scrapping, crawler, crawling, python, beautifulsoup, requests, selenium]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true

image:
    path: "https://soax.com/hs-fs/hubfs/Web%20Crawling%20vs%20Web%20Scraping%202.png?width=1920&height=1080&name=Web%20Crawling%20vs%20Web%20Scraping%202.png"
    alt: 

---

살다보니 웹 크롤링을 해야할 일이 생겼다

이것저것 검색하다보니 웹 크롤링과 웹 스크래핑 두 단어가 눈에 띄었다

처음에는 같은 뜻인줄 알고 뒤져봤으나 더 자세히 찾아보니 차이가 있었다


# 1. 웹 크롤링(Crawling)과 웹 스크래핑(Scraping)의 차이?
---

- 웹 크롤링은 동적으로 여러 웹페이지를 돌아다니면서 수집하는 것 

- 위키피디아에서는 웹 크롤링을 `웹을 체계적으로 검색하고 일반적으로 웹 인덱싱을 목적으로 검색 엔진에서 작동하는 인터넷 봇`으로 정의하고 있다

- 웹 스크래핑은 특정 웹페이지에서 필요한 데이터를 추출해내는 것

- 위키피디아에서는 웹 스크래핑을 `웹 사이트에서 데이터를 추출하는데 사용되는 데이터 스크래핑`으로 정의하고 있다

- 간단히 정리하면 넓고 광범위하게는 크롤링, 심도깊고 자세히는 웹 스크래핑으로 생각하면 될 듯

#  2. Selenium 과 Beautiful Soup
---

- 둘 다 Python에서 사용되는 웹 크롤링 라이브러리지만 각각의 장단점이 있음

![BeautifulSoup](https://lh3.googleusercontent.com/o9HtAcCnpfW_o5b1lkhvrJ0lzZBJ6Lm8TwxYue4Z3K5OdekeptiGVAUEPcBC_1ra7cFqAV0QOFByNl3ub_1BJbNe3A=w640-h400-e365-rj-sc0x00ffffff)

### 1. Beautiful Soup

- HTML과 XML 문서를 파싱하여 데이터를 추출하는데 사용되는 라이브러리

- HTML문서를 파싱하여 트리 구조로 만들어주고, 이를 통해 원하는 데이터를 쉽게 추출 가능

- 정적인 페이지에서만 사용 가능. HTML 문서를 분석하는 역할

![Selenium](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/Selenium_Logo.png/1200px-Selenium_Logo.png){: width="250"}

### 2. Selenium 

- 웹 브라우저를 자동으로 조작하여 스크래핑 하거나 테스팅하는 라이브러리

- 웹 브라우저를 직접 실행하여 페이지를 로드하고, 버튼 클릭, 스크롤 등을 수행할 수 있음

- 동적 페이지에서 주로 사용

- beautifulsoup에 비해 메모리 사용량이 많고 느리다고함

# 3. 결론

- 동적인 웹 페이지나, Javascrpit를 포함하는 페이지를 스크랩하는 경우 `Selenium` 사용함

- 정적인 콘텐츠를 가져오고 분석하는데는 `Requests + Beautiful Soup`를 사용함

- 두개 동시에 써서 효과적으로 추출할 수도 있다고 한다

