---
title: "AWS TechCamp : Bedrock과 API Gateway"
date: 2025-03-26 18:00:00 +0900
categories: [IT, Web]  
tags: [aws,bedrock, gateway]    
toc: true
comment: false
published: true
image:
    path: "https://img.youtube.com/vi/_vdK5PgcNvc/maxresdefault.jpg"
---

## Amazon Bedrock이란?
---

- AWS에서 제공하는 대규모 언어 모델 개발 및 배포 플랫폼
- 기존에는 GPT-3나 PaLM과 같은 대규모 언어 모델을 개발하려면    
막대한 컴퓨팅 자원과 전문 지식이 필요했으나, 베드록을 사용하면 누구나 쉽게 AI 모델을 구축하고 배포할 수 있다

## Bedrock 주요 기능
---

1. 사전 학습된 대규모 언어 모델 제공
2. 간편한 모델 미세 조정(Fine-tuning)
3. 유연한 모델 배포 옵션
4. 다양한 AI 활용 사례 지원

- 생성형 AI의 도전 과제
	- 출력 결과의 신뢰성
	- 데이터 보안
	- 규제 준수와 거버넌스

- 생성형 AI 애플리케이션의 요청 흐름
	1. prompt 사용자로부터 입력을 받음
	2. 앱이 사용자 입력 및 고객 데이터를 프롬프트로 변환
	3. 생성된 프롬프트를 모델에 전달
	4. 모델 응답 수신
	5. 사용자에게 응답 전송
	
## Amazon API Gateway를 활용한 안전한 생성형 AI 애플리케이션 구성
---

- 아마존 API Gateway는 개발자가 어떤 규모에서든 API를 쉽게 생성, 게시, 유지 관리, 모니터링 보호할 수 있게 해주는 완전관리형 서비스
- 사용자 인증, 인가, 감사 및 모니터링, 암호화, 외부노출 최소화 및 취약점 관리
- AI 정책에 따른 유해 컨텐츠 필터링, 민감 정보 삭제, 특정 주제 허용, 차단

## Amazon Bedrock 기반 책임감 있는 생성형 AI 구성 및 데모 
--- 

[실습 페이지 링크](https://catalog.us-east-1.prod.workshops.aws/workshops/e5ce2f2a-e576-41cc-838d-4b22da07c67d/ko-KR)

- Amazon Bedrock 가드레일
- 애플리케이션 요구 사항과 책임있는 AI 정책에 따라 맞춤화된 보호 기능 구현
- 콘텐츠 필터, 단어 필터
- 자동 추론

## RAG 및 GraphRAG 마스터하기
---

- RAG(Retrieval-Augmented Generation) : 대규모 언어 모델의 출력을 최적화하여 응답을 생성하기전에 학습 데이터 소스 외부의 신뢰할 수 있는 지식 베이스를 참조하도록 하는 프로세스

- 전통적 파인 튜닝 문제를 해결하기 위한 방법

벡터 거리 검색 방법 : ANN(Approximate Nearest Neighbor) 알고리즘
- 인덱스를 더 효율적으로 재구성
- 검색 가능한 벡터의 차원을 줄여 속도를 높임
- 정확도를 희생하고 검색속도가 크게 향상
- 수십만개 이상 벡터 검색 시 사용
- Graph-based(HNSW), Cluster-based(IVF) 를 가장 많이 씀
- nmslib, Lucene, faiss 라이브러리 형태로 제공


[RAG & Graph RAG 구현하기](https://catalog.us-east-1.prod.workshops.aws/workshops/9d4a1859-434f-4f79-902b-dd1ad020adef/ko-KR)

이쪽 백엔드는 다뤄본적이 없어서 이해하는데 애먹었다

추후 다시 확인하고 정리할 예정