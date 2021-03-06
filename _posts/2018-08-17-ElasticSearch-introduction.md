---
layout: post
title: "[Elastic search] 엘라스틱 서치 소개 및 특징"
subtitle: "엘라스틱 서치 알고 쓰자"
date: 2018-08-17 11:30:00
categories:
- Elastic search
tags:
- ElasticSearch
---
# 엘라스틱 서치
## 엘라스틱 서치 사용 사례
* 위키피디아: 실시간 타이핑 검색, 추천검색어 기능
* 더 가디안: 실시간 응대, 기사반응 분석
* 스택오버플로우
* 깃허브: 소스코드 검색
* 골드만 삭스: 주식시장 변동분석

## 특징은 무엇인가?
### 아파치 루씬 기반
아파치 루씬은 Java기반의 검색라이브러리이다. 현재는 엘라스틱서치를 비롯해서 솔라, 티카와 같은 프로젝트를 통해 널리 알려져있다. 아파치 루씬은 검색엔진으로 사용자 위치 정보, 다국어 검색, 자동완성등 다양한 기등을 지원한다. 엘라스틱 서치는 자바기반이기 때문에 1.1.x 이하는 자바 6이상의 버전이 설치돼 있어야하고, 1.2.x부터는 자바 7이상의 버전이 설치 돼 있어야한다.

### 실시간 분석
데이터는 색인 작업이 완료됨과 동시에 바로 검색할 수 있다.

### 분산 시스템
엘라스틱 서치는 여러개의 노드로 구성되는 분산 시스템이다. 노드는 데이터를 색인하고 검색기능을 수행하는 엘라스틱서치의 단위프로세스이다.

### 높은 가용성
각 노드는 데이터 원본과 복사본을 갖고있어 서로 다른위치에 저장, 노드가 실패하면 다른노드로 데이터를 옮겨 보존한다. 이는 높은 가용성과 안정성을 보장한다.

### 멀티 테넌시
데이터는 여러개의 분리된 인덱스 그룹으로 저장된다. 인덱스는 관계형 DB모델에서의 데이터베이스로 대응, 하지만 데이터 검색시 별도의 컨넥셔을 생성하는것이 아닌 서로 다른 인덱스의 데이터를 하나의 질의로묶어 검색하고 출력한다.

### 전문검색

### JSON문서 기반
엘라스틱 서치는 모든 필드가 색인돼 JSON구조로 저장된다. 따라서 모든레벨의 필드접근이 쉽고, 매우 빠른 속도로 검색된다.

### RESTFul API
덕분에 URI를 사용한 동작이 가능하다. HTTP프로토콜로 JSON문서의 입출력과 제어가 가능하다.

## 데이터 색인
![elasticsearchlogic](https://user-images.githubusercontent.com/33022147/44248333-dfad5080-a224-11e8-8578-12888a75ff25.PNG)
아파치 루씬은 색인절차를 거쳐 입력된 텍스트를 토큰으로 변환해 역파일 색인이라는 구조로 저장한다. 쉽게 말해 책의 맨뒤에 찾아보기 페이지를 역파일 색인이라고 생각할 수 있다.
