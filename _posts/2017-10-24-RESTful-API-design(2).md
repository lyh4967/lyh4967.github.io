---
title: "[Spring] RESTFul API 디자인(2)"
date: 2017-10-24 11:30:00
categories:
- Spring
tags:
- Spring
- RESTFul
---
**REpresentational
State
Transfer**
Http URI + Http Method URI와 Method를 폭넓게 활용하자!!

* 기존의 웹서비스
`http://hostname/agent.jsp?id=1`
* REST 개념 적용
`http://hostname/agent/1` -GET
>위에 꺼는 `@RequestParam`, 아래꺼는 `@PathVariable` 로 각각 읽어올 수 있다.

<br>

기존의 웹서비스 방식은 `@RequestParam` 으로
RESTful은 `@Pathvariable` 로 각각 읽어올 수 있다.
이제는 브라우저 뿐만 아닌 TV, Mobile등등 많은기기에 데이터를 전송해야 한다.
<span style=color:red>기존 URI에는 너무많은 정보들이 들어가 있어 간경화하는 것이 주 목적이다</span>
서버에서 application의 상태를 관리하지 않아 session이 없다.

# Restful API 가이드라인
## 동사는 사용하지 않는다. 명사만 사용할 것!(HttpMethod를 동사 대용으로 사용)
ex) /replies -PUT
<br>

## 복수형을 사용하자
URL은 복수형을 사용한다!
<br>

## 추상적이지 않고 구체적인 단어들이 좋다.
ex) things->animals->pigs->whildPigs
<br>

## 실제로 써보니 여러가지 난감한 상황이....
-그룹 경로 구성을 먼저 하자.

![RESTEx img](/image/2017-10-24/RESTEx.png)

<br>

|Resounce|POST|GET|PUT|DELETE|
|:------:|:-----------:|:-----------:|:-:|:---:|
|/group  |새로운그룹추가|최상위 그룹 리스트 조회|없음|없음|
|/group{path}|없음|{path}에 해당하는 그룹 정보 조회|{path}에 해당하는 그룹 정보 업데이트|{path}에 해당하는 그룹 삭제(하위 그룹 포함)|

* 각 그룹에 학생들은 어떻게 추가하지?
* 하위 그룹들은 어떻게 가져오지??

|Resounce|POST|GET|PUT|DELETE|
|:------:|:-----------:|:-------------------:|:-:|:---:|
|/group  |새로운그룹추가|최상위 그룹 리스트 조회|없음|없음|
|/group{path}|없음|{path}에 해당하는 그룹 정보 조회|{path}에 해당하는 그룹 정보 업데이트|{path}에 해당하는 그룹 삭제(하위 그룹 포함)|
|/group/{path}/item|새로운 학생 추가|없음|없음|학생삭제(학생 정보는 JSON으로 받음)|
|/group/{path}/childs|없음|자식 그룹 리스트 조회|없음|없음

요구사항 반영한 API

**아직까지 RESTful API를 이용해 완벽히 design을 해내는 사람은 드물다. 직접 설계해봐야 감이 잡힐것이고, 포트폴리오 홈페이지 만들때 직접 설계해 적용해봐야겠다!!!**
