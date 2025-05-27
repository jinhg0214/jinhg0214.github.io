---
title: "MCP : 윈도우에서 Obsidian 연동하기"
date: 2025-05-28 00:39:00 +0900
categories: [IT, Ai]  # 최대 2개 가능
tags: [claude, anthropic, mcp, obsidian]     
toc: true
comment: false
published: true
image:
    path: "https://i.ytimg.com/vi/VeTnndXyJQI/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLCD0uMkCf-8gA53IkQSfFv2Xm0hOA"
    alt: 
---

이전 글 [MCP 소개 글](https://jinhg0214.github.io/posts/mpc1/)에 이어서 

마크다운 노트앱인 옵시디언을 클로드와 연동하는 법을 알아보겠다

---

## 1. Claude Desktop 설치
![desktop](https://claude.ai/_next/image?url=%2Fimages%2Fhome-page-assets%2Fmac_ui.png&w=1200&q=75)

[Claude Desktop](https://claude.ai/download)

Claude Desktop 받아서 설치하기

현재 클로드 웹사이트에서는 MCP 서버를 지원하지 않으므로, 직접 로컬 환경에 설치하여 사용해야함

## 2. Node.js 설치

![nodejs](https://nodejs.org/static/logos/nodejsLight.svg)

[Node.js](https://nodejs.org/ko)

서버가 Node.js에서 구축되므로 설치해야함

## 3. MCP 서버 설정하기

`claude_desktop_config.json` 파일을 수정하는 방식으로 진행

`%APPDATA%\Claude\claude_desktop_config.json` 경로에 파일 생성하기

### 3-1. 설정 파일 세팅

위에서 만든 파일을 열어 `server-filesystem` MCP 서버를 추가하기 

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": [
        "-y",
        "obsidian-mcp",
        "C:\\Users\\User\\Documents\\mcp" 
      ]
    }
  }
```
다음과 같이 작성하여 저장하면, 클로드가 컴퓨터 내부의 폴더를 읽고 쓸 수 있게 된다

`"C:\\Users\\User\\Documents\\mcp" `와 같이 작성된 부분이 클로드가 사용할 볼트의 경로이므로, 

이를 지정해준다

중요!! 옵시디언이 파일을 읽고 **삭제**할수도 있음!!!

## 4. Filesystem MCP 사용해보기

위에까지 세팅했다면, 클로드가 해당 폴더 내에 있는 파일을 읽고 쓸 수 있게 됨

![image1](assets\img\posts\20250528_1.png)

파일 시스템을 읽고 쓸 때 허용 요청을 보낸다

![image2](assets\img\posts\20250528_2.png)

![image3](assets\img\posts\20250528_3.png)

MCP 만 체크해달라고 했더니 

클로드 지혼자 뚝딱뚝딱 옵시디언과 연동기능을 체크하는 모습

좀 무서움

파일 읽기 쓰기, 내용 읽고 요약 등 다양한 작업들을 수행한다

![image4](assets\img\posts\20250528_4.png)

질문을 몇개 안했는데도 금방 사용량 소진함

---

## 참조

[https://chinensis.tistory.com/](https://chinensis.tistory.com/entry/%EC%B4%88%EB%B3%B4%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-MCP-%EC%84%9C%EB%B2%84-%EC%82%AC%EC%9A%A9-%EA%B0%80%EC%9D%B4%EB%93%9C-%ED%81%B4%EB%A1%9C%EB%93%9C%EA%B0%80-%EB%82%B4-%EA%B0%9C%EC%9D%B8-%ED%8C%8C%EC%9D%BC%EA%B3%BC-%EC%9C%A0%ED%8A%9C%EB%B8%8C-%EC%98%81%EC%83%81%EC%9D%84-%EB%B6%84%EC%84%9D%ED%95%A0-%EC%88%98-%EC%9E%88%EA%B2%8C-%ED%95%B4%EB%B3%B4%EC%9E%90)

[https://orangecake.tistory.com/26](https://orangecake.tistory.com/26)
