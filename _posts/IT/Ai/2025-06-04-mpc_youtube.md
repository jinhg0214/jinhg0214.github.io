---
title: "MCP : YouTube MCP Server로 유튜브 영상 요약하여 응용하기"
date: 2025-06-04 11:00:00 +0900
categories: [IT, Ai]  # 최대 2개 가능
tags: [claude, anthropic, mcp, youtube, summary]     
toc: true
comment: false
published: true
image:
    path: "https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4b17y%2FbtsNcWvmGdK%2F6EWKtCRTVRdRVWVyNJ9FSK%2Fimg.png"
    alt: 
---

MCP 서버를 살펴보던 중 유용해 보이는 서버가 있길래 사용해봄

YouTube 영상의 내용을 Claude나 다른 AI와 함께 분석할 수 있는 툴이다

[adhikasp의 YouTube MCP Server](https://github.com/adhikasp/mcp-youtube)를 사용하면 

유튜브 영상의 자막을 쉽게 가져와 LLM과 연결할 수 있다

이 요약한 내용을 다른 MCP 서버에 또 가져다 쓸 수 있음 

---

## YouTube MCP Server란?

Model Context Protocol을 사용하여 YouTube 동영상의 자막을 다운로드하고 

LLM에 연결할 수 있는 서버

이를 통해 긴 영상의 내용을 요약하거나, 특정 주제에 대한 분석을 수행할 수 있다

### 주요 기능

- YouTube 동영상에서 트랜스크립트 자동 다운로드
- 동영상 ID와 전체 YouTube URL 모두 지원
- 트랜스크립트에 타임스탬프 포함으로 정확한 시점 파악
- Claude Desktop 등 MCP 호환 클라이언트와 완벽 연동

### 주의사항

- **자막 필수**: 영상에 자막(자동 생성 포함)이 있어야 사용 가능
- **공개 영상만**: 비공개나 삭제된 영상은 접근할 수 없음
- **지역 제한**: 일부 지역 제한 영상은 가져오지 못할 수 있음
- 긴 영상의 경우 구체적인 질문을 준비하면 더 정확한 답변을 받을 수 있음
- 타임스탬프를 활용하면 더 구체적인 답변을 받을 수 있음

---

## 설치 및 설정 방법

윈도우 환경에서 클로드 데스크탑 기준으로 설명함

### 1. uvx 설치

터미널에 `pip install uvx`를 입력하여 uvx를 설치한다

### 2. MCP 클라이언트 설정

MCP 클라이언트의 설정 파일에 다음과 같이 추가함

```json
"mcpServers": {
  "youtube": {
    "command": "uvx",
    "args": ["--from", "git+https://github.com/adhikasp/mcp-youtube", "mcp-youtube"]
  }
}
```

### 3. 환경변수 설정 (필요시)

`"command": "uvx"`로 설정할 때 환경변수에 uvx가 추가되지 않았다면 오류가 발생할 수 있음

환경변수 추가 방법
1. 터미널에서 `where uvx`로 uvx 설치 경로 확인
2. 환경변수 추가시 이름은 `uvx`, 경로는 위에서 검색한 경로를 추가합니다

---

## 실제 사용

![img1](assets\img\posts\20250604_1.png)

설정이 완료되면 Claude Desktop에서 바로 위 짤처럼 유튜브 영상을 분석할 수 있다

기존의 ChatGPT에서 사용하던 자막 분석과 유사한 방식을 이용하지만, 

MCP를 이용해서 별도의 웹 서비스나 확장 프로그램, 로그인 없이 더 쉽게 사용할 수 있게 되었다

### YouTube MCP Server의 장점

1. 완전한 통합 환경 - 별도 도구 없이 Claude Desktop에서 바로 분석 가능
2. 로컬 처리 & 프라이버시 - 기업이나 개인이 민감한 콘텐츠를 분석할 때 외부 서비스를 거치지 않음
3. 확장성 - 한번 분석한 영상 정보들을 다른 MCP 서버들에 사용 가능

---

## 마무리

유튜브를 요약해주는 AI는 이미 기존에도 많이 사용하는 추세였다

하지만 대부분 영상을 요약하는데 그치거나, 결과를 복사해서 다른 도구에 붙여넣는 번거로움이 있었다

이제 MCP라는 강력한 도구 덕분에 단순히 요약만 받는것이 아니라,

AI가 직접 영상 내용을 이해하고 활용할 수 있는 경지에 이르렀다