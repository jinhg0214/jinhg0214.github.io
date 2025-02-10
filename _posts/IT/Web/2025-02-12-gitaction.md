---
title: "GitHub Action"
date: 2025-02-12 10:52:00 +0900
categories: [IT, Web]  # 최대 2개 가능
tags: [git, gitpage, gitaction, cicd]     # 태그는 항상 소문자로 작성할 것
toc: true
comment: false
published: true
image:
    path: "https://user-images.githubusercontent.com/17984781/231239788-419ee9b0-0211-40f9-9d0a-d192db6718bd.png"
    alt: 
---

GitAction에 대한 사전 이해 없이 CI/CD를 구축하려니 모르는 개념이 너무 많아

한번 공부할겸 정리한 내용

## 1. 개요
---

- GitHub 플랫폼에서 제공하는 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD(연속 통합 및 지속적인 업데이트) 플랫폼
	- CI(Continuous Integration) : 지속적 통합
		- 추가, 변경된(push, pull req 등) 코드를 빌드하고 테스트하는 프로세스를 자동화 하는 것
		- 이후 코드를 merge
		- 코드 규칙 Lint를 잘 지켰는지, 제대로 동작하는 지 등 체크
	- CD(Continuous Delivery,  Deployment) : 지속적 제공 및 배포
		- Delivery : CI 과정 이후 병합하여 실행 테스트를 할 수 있도록 하는 것
		- Deployment : 배포 과정을 자동으로 처리
- 쉽게 말해서 반복적인 빌드, 테스트, 배포 작업을 자동으로 처리해줌
- 코드의 통합과 배포 프로세스를 자동화하여 개발 생산성을 높일 수 있다

## 특징
---

>- 컨테이너(도커) 기반으로 동작
>- Workflow 를 작성하여 다양한 이벤트를 기반으로 실행시킬 수 있음
>- Workflow는 Runners라 불리는 인스턴스에서 Linux, MacOS, Windows 환경에서 실행됨
>	- 한번에 여러 운영체제에서 테스트 가능
>- Workflow를 직접 만들고 공유할 수 있음
>- YAML로 작성

### 장점
>- 다른 CI/CD 툴(jenkins)처럼 서버 설치가 필요하지 않음
>- 비동기적 병렬 실행 가능
>- Github에서 마켓 플레이스로 workflow를 쉽게 공유하고 사용할 수 있음

### 단점
>- 캐싱이 필요한 경우, 자체 캐싱 로직을 작성해야함
>- 서버에 장애가 일어나거나 리소스를 초과할 경우, 개발자가 직접 문제를 해결해야함

## 2. 구성 요소
---

![compo](https://docs.github.com/assets/cb-25535/mw-1440/images/help/actions/overview-actions-simple.webp)

### Workflows, 워크플로
>- 자동화된 프로세스가 정의되어 있는 하나의 파일
>- Github Action에서 가장 최상위 개념
>	- YAML 파일로 작성됨
>	- 해당 파일을 트리거할 규칙, 실행할 동작들이 작성되어있음
>	- 각 작업은 자체 가상 머신 '실행기' 또는 컨테이너 내에서 실행되며, '단계'를 하나 이상 포함함
>- 리포지토리의 **이벤트로 트리거 될 때** 실행되거나 **수동**으로 또는 **정의된 일정**에 따라 트리거 가능
>- `.github/workflows`디렉터리에 정의됨

### Runner, 실행기
>- 트리거될 때 워크플로를 실행하는 서버
>- 클라우드에서 동작하므로, 서버가 별도로 필요하지 않음
>- 각 실행자는 한 번에 하나의 작업을 실행할 수 있음
>- GitHub은(는) **워크플로**를 실행할 Ubuntu Linux, Microsoft Windows, macOS 실행기를 제공
>	- 각 워크플로는 새로 프로비저닝 된 새 가상 머신에서 실행됨

### Event, 이벤트
>- 워크플로 실행을 트리거하는 리포지토리의 특정 활동
>	- pull, open issue, commit push 등
>	- 일정 또는 수동 실행 가능

### Jobs, 작업
>- 워크플로 내에서 실행될 명령
>- Event로 Workflow가 실행되면 Job에 작성된 명령들이 실행됨
>- 각 단계가 실행되는 셸 스크립트 또는 실행되는 작업
>- 순서대로 실행되며 서로 종속됨
>- 동일한 실행기에서 실행되므로 각 단계간 데이터 공유 가능
>- Jobs는 자신의 환경 설정과 Steps를 가짐

### Steps, 단계
>- Jobs 내에 steps 명령어 안에 존재하며, 여러개의 step들로 구성됨
>- 각 스텝들은 script, 명령어 또는 action을 실행할 수 있다
>- 각 step들은 데이터를 공유할 수 있다

### Actions
>- 복잡하지만 자주 반복되는 태스크를 수행하는 GitHub Actions 플랫폼용 사용자 지정 애플리케이션

## 3. Github Action 명령어 사용법 예시
---

각 레포지토리 최상단에 `.github/workflows` 디렉터리를 만들고

그 내부에 이름을 `.yml`파일을 만들면 workflow 파일 생성 완료

```yml
name: workflow test
on:
	push:
		branches:
			- main
jobs:
	workflow-test-job:
		runs-on: ubuntu-latest
		steps:
			- name: Checkout repo
				uses: action/checkout@v3

			- name: Use Node.js
				uses: actions/setup-node@v3
		with:
			node-version: '14'
			
			- name: Install npm
				run: npm install
```
> - name : workflow의 이름 설정
> - on: workflow를 실생 시킬 이벤트 정의
> 	- 위의 예시는 main 브랜치에서 push가 일어날 때 발동
> - jobs : Event에 부합했을 때 실행 시킬 작업 정의
> 	- 바로 밑에 해당 job의 이름 정의
> 		- 설명 및 어떤 환경에서 동작시킬것인지
> - steps : 각 스텝마다 name 정의 및 uses(마켓에 있는 action 가져오기)
> 	- market에서 가져오는 경우 `{owner}/{repo}@{ref|version}`의 형태
> 	- run 은 직접 명령어 실행

## 참조
---

- [Git Action 공식문서](https://docs.github.com/ko/actions/about-github-actions/understanding-github-actions)
- [# Github Action과 필요한 개념정리](https://velog.io/@rokwon_k/Github-Action%EA%B3%BC-%ED%95%84%EC%9A%94%ED%95%9C-%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC)