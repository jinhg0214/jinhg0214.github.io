---
title: "AWS TechCamp : VPC, EC2, DB 그리고 S3"
date: 2025-03-25 19:53:00 +0900
categories: [IT, Web]  
tags: [aws, vpc, ec2, database, s3]    
toc: true
comment: false
published: true
image:
    path: "https://d1.awsstatic.com/apac/events/2024/kr/summit-seoul/aem-event-kr-banner-aws-techcamp.a59f6ff2bfa94999968317f82d8989b6acc4d474.png"
---

2025 3월 AWS TechCamp에 참여하면서 학습한 내용들 정리

[AWS TechCamp 소개 페이지](https://aws.amazon.com/ko/events/seminars/aws-techcamp/)

[세미나 및 동영상 다시보기 페이지](https://kr-resources.awscloud.com/aws-techcamp)

[실습 페이지](https://catalog.workshops.aws/general-immersionday/ko-KR)

## AWS TechCamp란?
---

![Image](https://github.com/user-attachments/assets/0d8666c3-5d80-4286-922e-c30ce410de3b)

간단히 말해, AWS 기술들을 실습을 통해 경험을 쌓을 수 있음

온라인 세션은  분기별로 한번씩 총 4번 열리는데, 이 기간동안 3일간 진행되며

클라우드 서비스가 생소한 사람들을 위한 기초과정 (레벨 100)과

클라우드 기본 서비스 교육이 필요한 사람들을 위한 기본 과정 (레벨200)으로 이루어져있음

![Image](https://github.com/user-attachments/assets/b4bd130f-0ee4-415e-a287-ad34df726a15)

오프라인 세션은 특정 인더스트리에 대한 주제로 대상 집중 세션으로 구성된 기본/심화 (레벨200-300)이 진행됨

이번에 참여한 오프라인 세션은 다음과 같은 주제로 진행되었다

![Image](https://github.com/user-attachments/assets/330bf76a-5c72-4ca7-a683-35e2bc2e1414)


## 1일차
---

오전 09~12와 오후 14~17 으로 나눠서 진행하였다

오전에는 크게 두가지 세션이 진행되었다

1. 클라우드의 시작, AWS로 배우는 첫걸음

2. 핵심 서비스로 배우는 AWS (VPC, EC2)

### 1. 클라우드의 시작, AWS로 배우는 첫걸음
---

온 프레미스(On-Premises)와 클라우드의 차이

클라우드의 장점, AWS의 장점을 다루는 30분짜리 강연

### 2. 핵심 서비스로 배우는 AWS (VPC, EC2)
---

VPC와 EC2에 대한 이론 교육과 실습 교육이 진행되었다

네트워크와 컴퓨팅 분야의 활용방법과 이해도를 높이는 강연이였다

먼저 VPC에 대한 교육을 한시간, 이후 EC2에 대한 이론 교육을 한시간 진행하였다

#### Networking in AWS

VPC에 대한 교육 진행

- AWS 리전 

AWS Region은 전세계에 AWS 데이터센터가 클러스터 형태로 위치한 물리적 장소

하나의 리전은 고가용성, 확장성과 내결함성을 위해 3개 이상의 가용 영역(Availability Zone)으로 구성됨

- 가용 영역

모든 가용 영역은 AWS 리전의 중복 전력, 네트워크 및 연결이 제공되는 하나 이상의 개별 데이터 센터로 구성됨

![Image](https://github.com/user-attachments/assets/697d966f-094e-46d0-89cc-fe837ff19a35)

다음 그림과 같이 리전 내에 3개 이상의 AZ로 구성되고, 이 AZ 내에 데이터 센터 랙, 호스트로 구성됨

- Amazon Virtual Private Cloud (VPC)

AWS 클라우드에서 사용자가 정의한 논리적으로 격리된 가상의 네트워크

IP 주소, 서브넷, 네트워크 토폴로지, 라우팅 규칙, 보안 규칙 등을 설정 가능함

VPC는 여러 AZ에 걸쳐서 생성할 수 있으며, 

이를 통해 하나의 인스턴스에 문제가 생겨도, 다른 가용영역의 인스턴스를 사용하여 안전성을 보장함

![Image](https://github.com/user-attachments/assets/d9e26c3a-c47e-4e91-80a4-bf3e35b1dc81)

EC2, Lambda, RDS, Redshift 가 VPC내부에 위치하고, DynamoDB, S3는 VPC 외부에 위치한다

즉, VPC 내부의 서비스들은 사용자 규칙에 영향을 받으나, VPC 외부 서비스들은 가용 영역의 영향을 받음

VPC는 한번 세티앟면 수정이 어려우므로, 초기 세팅시 주의해야한다

IP 주소는 CIDR 표기법을 따른다

Public Subnet은 트래픽이 IGW로 향하는 경로를 설정한 서브넷. 

웹서버나 인터넷과 닿은 ELB등이 배치할 수 있는 DMZ 인프라로 유용함

Private Subnet은 트래픽이 IGW로 향하는 경로가 없는 서브넷

Public Subnet을 경유하여 인터넷을 사용 가능. 

서버나 DB같이 외부에서 닿을 수 없도록 해야하는 리소스들을 배치하는데 유용함

VPC는 Internet Gateway가 있어야 인터넷과 통신할 수 있다. 한개의 VPC에는 하나의 IGW만 연관 가능

이후 실습을 진행하였는데 다음 링크에서 진행하였다

[https://catalog.workshops.aws/general-immersionday/ko-KR](https://catalog.workshops.aws/general-immersionday/ko-KR)

해당 링크의 워크샵을 통해서 메뉴얼을 보고 따라하면서 직접 세팅해보는 방식으로 진행하였다

개인 AWS 계정을 통해 진행하였음

#### Compute in AWS

Amazon EC2 개요

![Image](https://github.com/user-attachments/assets/02cc106d-247c-4ff6-bf04-d64d8f0458d4)

다양한 프로세서와 아키텍쳐, 운영체제를 지원함

운영체제 라이센스 비용은 알아서 처리해줌

자신의 환경에 맞는 EC2 인스턴스 유형을 선택하여 비용을 절감할 수 있다

AMI, ,사용자 데이터, 시작 템플릿, EC2 관련 자격증명, 로드밸런싱 등 컴퓨팅 관련 기능 및 서비스를 제공함

실습은 마찬가지로 워크샵 페이지에서 진행하였다

EC2를 생성하고, 로드밸런싱을 설정하고 부하테스트를 진행하였다

실습 후 꼭! 삭제할것!!


### 3. Database on AWS
---

데이터베이스의 개념과 종류, 그리고 AWS RDS, Aurora, DynamoDB에 대한 설명

AWS DB의 장점

클라우드 마이그레이션을 돕는 다양한 방법에 대해 설명 후 

DB 실습을 진행하였다


### 4. Storage on AWS
---

스토리지의 개념, 기존 스토리지 방식의 한계

AWS 클라우드 스토리지의 장점

스토리지를 위한 서비스들

- Amazon Simple Storage Service (S3) 
- AWS 블록 스토리지
- Amazon EC2 인스턴스 스토어
- Amazon Elastic File System
- AWS Backup
- 데이터 전송
- AWS DataSync

에 대한 설명 후, 실습을 진행하였다

사용 후 꼭 삭제해야 요금폭탄을 안맞는다!!