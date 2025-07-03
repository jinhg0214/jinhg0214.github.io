---
title: "MCP : YouTube MCP Server로 유튜브 영상 요약하여 응용하기"
date: 2025-07-03 22:37:44 +0900
categories: [IT, Ai]  # 최대 2개 가능
tags: [google, gemini, cli, tools, automation]     
toc: true
comment: false
published: true
image:
    path: "https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Gemini_CLI_Hero_Final.width-1300.png"
    alt: 
---

## 다시 터미널로. 개발자들을 위한 Google의 선물. Gemini CLI 

개발자라면 누구나 하루의 대부분을 터미널과 코드 에디터 앞에서 보낸다.

이 터미널에서 강력한 AI 모델을 사용할 수 있다면 어떨까? 무려 무료료 말이다

구글이 최근 Gemini CLI 버전을 무료로 공개했다

![pro](https://meetcody.ai/wp-content/uploads/2025/03/final_2.5_blog_1.original-1024x629.jpg)

이 CLI는 강력한 **Gemini 2.5 Pro** 모델을 기반으로 하며, 일부 벤치마크에서 GPT-4를 능가하기도 한다.

일일 요청 1000회, 사실상 무료로 풀어버렸다

(무료 버전은 사용자의 정보가 학습에 사용된다고 하니 찜찜한 사람은 결제해서 사용하면 될듯 함)

이 글에서는 Gemini CLI가 기존 AI 도구들과 어떻게 다른지, 어떻게 설치하고 사용하는지, 솔직한 후기 등에 대해 담아보겠다

---

## 1. 기존 AI와의 차이점?

많은 사람들이 ChatGPT, Claude와 같은 AI 챗봇들에 이미 익숙해져 있다. 

어떤 개발자들은 여러 AI 챗봇들을 띄워두고 동시에 같은 질문을 보낸 뒤에, 

가장 성능이 좋은 답변을 사용하는식으로 개발하는것도 봤다

이러한 과정에서 코드를 복붙하고, 질문에 대한 연관 자료들을 제공하는 등 번거로운 '컨텍스트 전환'이 필요했다

GEMINI CLI 는 이 문제를 근본적으로 해결했다

1. 프로젝트 전체 컨텍스트 이해
	- 텍스트 입력뿐만 아니라, 프로젝트 폴더 내의 모든 파일과 구조를 스스로 파악함. 
	- 특정 함수에 대해 수정해달라고 요청하면, 그 함수를 사용하는 다른 파일들까지 분석하여 영향을 최소화 하는 코드를 제안함
2.  파일 시스템 직접 제어
	- 직접 파일을 읽고 쓰고, 검색할 수 있음
	- "src 폴더 내에 모든 .js 파일에서 'useState'를 사용하는 컴포넌트를 찾아줘" 같은 명령이 가능함
3. 터미널 명령어 연동
	- `run_shell_command` 기능으로 `git status`나 `npm test`같은 터미널 명령을 직접 실행해보고, 그 결과를 AI가 인지하고, 다음 작업을 알아서 수행할 수 있음
	- 테스트 실패 로그를 보고 자동으로 코드 수정을 제안하기도 함
4. IDE와의 통합
	- 터미널을 사용하는 IDE라면 어디든지 사용 가능한듯 함
	- VSCode, Pycharm, Vim에서도 사용가능한걸 확인함
	- 코드 수정과 터미널 명령을 하나의 창에서 쉽게 해결가능

---

## 2. 설치

### 1. Node.js 설치
 Node.js (https://nodejs.org/)에서 최신버전 (LTS 버전 권장)을 설치한다

### 2. GEMINI CLI 설치

1. `npm install -g @google/gemini-cli` 로 Gemini CLI를 설치하면 끝이다

![gemini](https://i.ytimg.com/vi/T76NbeTdDFA/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLCyaoEC-2jUKwHna7VkLlmlX90LJQ)

이후 터미널에서 `Gemini`를 입력하면 터미널이 실행됨

간단한 테마 설정과 구글 계정 연동을 설정하면 끝

---

## 3. 커스터마이징

### 1. `.gemini`폴더와 `GEMINI.md` 라는 파일을 통해 설정을 관리

```
* 구조:
1     my-simple-project/
2     ├── GEMINI.md
3     └── ... (다른 파일들)
```

- `GEMINI.md` : 단순히 AI에게 몇가지 규칙만 알려주고 싶은 경우 사용. 루트파일에 두면 됨
- 마크  다운 형식으로 적어두면, Gemini CLI가 추후에 다시 대화를 할 때 이 기록을 참조함
- "이 프로젝트는 React를 사용하고, Firebase를 사용합니다"
- "커밋 메시지 양식은  Conventional Commits 양식을 따라주세요 등"
- Gemini CLI가 자동으로 이 컨텍스트를 모든 요청에 적용합니다. 매번 같은 설명을 반복할 필요가 없어 프롬프트 길이를 아낄 수 있음

```
* 구조:
1     my-pro-project/
2     ├── .gemini/
3     │   ├── GEMINI.md
4     │   └── settings.json
5     └── ... (다른 파일들)
```
- `.gemini` : 구조가 복잡하거나, 체계적인 프로젝트에서 사용
	- Gemini CLI를 더 체계적으로 사용하고 싶을 때, "설정 보관함"처럼 사용함 
	- 프로젝트의 기술 스택, 코딩 규칙등을 `GEMINI.md`에 상세히 기술
	- 이 프로젝트에 최적화된 AI 모델이나 특정 설정을 `settring.json`에 정의하는 식
### 2. `settings.json`을 이용한 고급 설정 및 모델 연결

`settings.json` 파일을 통해 Gemini CLI의 동작 박식을 세밀하게 제어하고,

특정 모델이나 MCP 서버에 연결하여 사용할 수 있다

- 이 프로젝트에서 사용할 Gemini 모델
- API 키 관리
- MCP 서버의 정의 및 실행
- 기타 파라미터 제어 등

```json
{
	"model": "gemini-2.5-pro",
	"api_key": "YOUR_GEMINI_API_KEY",
	"mcpServers": {
	  "obsidian": {
		"command": "npx",
		"args": [
		  "-y",
		  "obsidian-mcp",
		  "C:\\Users\\User\\Documents\\gemini_obsidian"
		]
	  }
	}
}
```
위와 같은 `settings.json` 파일을 `.gemini` 폴더 내에 넣어두면 알아서 MCP 연동까지 알아서 된다

### 3. * 멀티 인스턴스로 여러 AI를 동시에 운영하기
  - 터미널 탭마다 각기 다른 모델, 다른 API 키, 다른 GEMINI.md 설정을 적용하여 여러 작업을 병렬로 처리할 수  있음
  - 예를 들어 한쪽 탭에서는 코드 리팩토링을, 다른 탭에서는 문서 번역을 동시에 시킬 수 있다

---

## 4. 사용 후기

### 👍 장점 (Pros)

1. 압도적인 컨텍스트 이해 능력
	- 여러 파일에 걸쳐있는 복잡한 코드를 이해하고 수정하는 능력이 상상 이상임
	- "YOLOv5s 커스텀 모델을 만들었는데 제대로 동작하는지 체크하고 싶습니다" 라고 질문해봄
	```
	1. 자기혼자 뚝딱뚝딱 yolov5s 테스트코드로 동작하는지 체크
	2. 안되니 numpy 버전, ultralytics, cv 버전체크
	3. 둘다 삭제하고 재설치했는데도 안되는걸 확인하고, 두 라이브러리만 import하는 테스트코드 작성 
	4. 이래도 안되니 cv_env 밀어버리고 재설치해도 되겠냐고 물어보더니 재설치까지 알아서 진행함
	
	나는 의사결정에서 확인만 눌러주면 됬음
	```
	
2. 브라우저와 IDE를 오가는 시간 낭비가 사라짐
	- 챗봇들을 사용할땐, 웹페이지나, 해당 프로그램을 켜놔서 상당히 어수선했는데
	- 코딩, 테스트, 디버깅, 문서화 모든 과정이 하나의 IDE에서 터미널로 수행 가능했음.
3. 강력한 자동화 잠재력
	- 쉘 스크립트 및 MCP와 연동하면 단순 반복 작업을 AI에게 맡겨 자동화 가능
4. 멀티 인스턴스 가능
	- 터미널 탭별로 다른 모델과 키를 지정하여 병렬 작업도 수행 가능함

### 👎 아쉬운 점 (Cons)
1. 미세한 한글 입력 지연
	- CLI 기반이다보니, 한글을 입력할 때 지연이 느껴질 때가 종종 있었음
	- 사용에 지장을 줄 정도는 아니지만, 끝 글자가 잘리는 경우가 있음
2. 채팅 히스토리 관리
	- ChatGPT처럼 대화 세션(채팅방)별로 히스토리가 관리되지는 않음
	- 창을 한번 닫으면 기록
	- 비슷한 기능을 구현하려면, 각기 다른 폴더에서 `.gemini`설정을 따로 관리하는 방식으로 사용해야함

---

## 5. 결론

개발자라면 한번쯤 찍먹해볼만한 AI 도구

단순한 AI 챗봇이 아니라, 개발 환경에 완벽히 통합된 AI 에이전트라고 생각함

특히 복잡한 프로젝트의 전체 구조를 파악하고, 터미널과 유기적으로 상호작용하는 능력은 기존 AI 도구들을 압도한다고 생각함

그러나 CLI 환경이라는점은 개발자가 아닌 일반 사용자들에게 가장 큰 걸림돌이 될 것이라고 생각함
