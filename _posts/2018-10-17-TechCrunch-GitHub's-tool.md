---
title: "[TechCrunch] 깃허브가의 실행 툴 Actions"
subtitle: "깃허브의 혁신 Action"
date: 2018-10-17 11:30:00
categories:
- Article
tags:
- Article
- TechCrunch
---

# Git과 SVN

![github_mark](https://user-images.githubusercontent.com/33022147/47083283-131f5200-d24b-11e8-8803-9f6ec1299476.jpg)
개발자들에게 대표적으로 사랑받는 형상관리 툴이었던 SVN은 점차 Git에게 자리를 내어주고 있다. 그렇다면 왜 Git으로 갈아타는 추세가 형성되었을까? Git이 SVN과 다른점은 분산형 관리 시스템으로 다양한 형태의 Workflow를 가능하게 한다는 것이다. 중앙서버에 소스코드와 히스토리를 저장하는 SVN과 달리 Git은 소스코드를 열 개발 PC와 저장소에 분산해서 저장하기 때문에 중앙 서버에 장애가 발생해도 로컬 저장소에 커밋을 할 수 있으며, 로컬 저장소들을 이용하여 중앙 저장소의 복원도 가능하다. 또한 분산형으로 코드를 관리하기 때문에 다양한 Workflow를 가능하게 한다.<br/>
Git은 SVN의 중앙 집중형 방식으로도 Workflow를 구성할 수 있다. 중앙에 저장소를 두고 각각의 개발자들이 clone하여 작업, 그리고 공유저장소에 Push하는 식으로 관리가 가능하다. 또한 분산형으로도 Workflow를 구성할 수 있다. 흔히 Github에서 오픈소스를 개발할 때 쓰는방식으로 Integration-Manager Workflow라고도 한다. 메인 저장소에 Integration-Manager이 권한을 관리하며 개발자들이 Clone해 놓은 자신의 저장소에 작업을 반영하고 메인저장소에 commit을 요청하면 요청을 Integration-Manager는 수정사항을 리뷰하여 적절할 경우 메인저장소에 변경사항을 반영한다.

# Github's workflow automation tool Actions

![github-action](https://user-images.githubusercontent.com/33022147/47083436-85903200-d24b-11e8-83c3-6f1153ceefc9.jpg)
소제목의 내용 그대로입니다. 이 툴을 통해 플랫폼에서 코드를 호스트 할수 있으며 뿐만 아니라 실행할 수도 있습니다. 실제로 GitHub의 플랫폼 책임자 Sam Lambert는 "GitHub의 역사에서 가장 큰 변화"라고 설명했습니다. 또한 그는 이렇게 이야기 했습니다. "사람들이 이제는 특정 응용 프로그램 및 프레임 워크에 대한 배포, workflow에서 최상의 빌드를 작성하는게 사실상 표준이 될것이다." 이는 저장소에서 코드를 Action으로 바꾸고 Github에서 실행할 수 있는것을 의미합니다. 이땐 Docker 파일을 만들고 이를 빌드해 workflow에 연결할 수 있습니다. 혹은 비주얼 편집기로도 사용가능합니다. <br/>
그리고 Githuub는 Action외에도 Github 플랫폼에 여러가지 새로운 기능을 추가로 발표했습니다.
///Github Connect 사진
그 중 하나가 Github Enterprise를 사용하는 개발자들에게 관리자가 공개 프로필의 일부로 개발자의 작업으 표시할 수 있는 기능입니다. Github가 많은 개발자들에게 포트폴리오로서 사용되기 때문에 더욱 중요합ㄴ디ㅏ. GitHub security Advisory API를 사용하면 개발자가 자동 취약성 검색을 통해 코드에서 스레들르 쉽게 찾을 수 있고 Java 및 .NET 프로젝트의 보안 취약성 경고를 주기도 한다는 것입니다.

<br/>
<br/>
출처: [Tech Crunch](https://techcrunch.com/2018/10/16/github-launches-actions-its-workflow-automation-tool/)
