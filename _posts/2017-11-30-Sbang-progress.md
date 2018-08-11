---
title: "[Sbang] 11/30(월) 비트프로젝트 진행상황"
date: 2017-11-30 10:10:00
categories:
- Sbang
tags:
- PROJECT
- SPRING
---
**현재 프로젝트 진행(스터디 프로세스)은 기본 80%진행 되었다.**

# 스터디 등록- 영훈(80/100)
* 카테고리 구상- 영광
* 파일업로드 : input:file로 기능구현 (90/100)

# 스터디 리스트 불러오기 - 영훈
* 검색기능(90/100) -> 자동완성, user와 연동문제
* 페이징처리(100/100)
* 썸네일 구현(90/100) : 차례대로 출력되도록

# 스터디 상세보기 구현 - 영광(90/100)

# 스터디 삭제 - 영광(100/100)

# 스터디 수정페이지 - 영광(100/100)

studyList.jsp 구현중 썸네일 이미지를 불러오는 과정에서 문제가 생겼다. 컨트롤러에서는 list로 모델에 값을 넘겨주지만 정작 자바스크립트에서는 java에서 쓰이는 List객체를 불러올 수 없기 때문이다. 이 문제는 나에게 많은 고민을 안겨주었다. 구글링 결과 list를 불러올때는 ajax로 따로 불러오는 것이 아닌 테이블 조인을 통해 한번에 불러오는 것이 바람직하다는 결론을 내렸다.
```html
<script>
	$(document).ready(function() {
    /*스터디리스트 썸네일과같이 추력  */
    var template = Handlebars.compile($("#templateList")
    		.html());

    /*리스트 페이지 썸네일과 같이 출력  --> 콜백순서대로 출력되니 정렬이 안되네*/
    <c:forEach items="${list}" varStatus="listIdx" var="studyVO">
    if ("${studyVO.imagePath}") {
    	var fileInfo = getFileInfo("${studyVO.imagePath}");
    } else {
    	var fileInfo = getFileInfo(" ");
    }

    var html = template(fileInfo);
    <fmt:formatDate value="${studyVO.studyRegDate}"
    pattern="yyyy-MM-dd" var="date" />
    var studyInfo = "NO: ${studyVO.studyNo}</br>"
    		+ "Name: <a href='/study/studyView${pageMaker.makeSearch(pageMaker.cri.page)}&studyNo=${studyVO.studyNo}'>${studyVO.studyName}</a></br>"
    		+ "RegDate: ${date}";
    $("#studyList-thumbnail").append(
    		html + studyInfo + "</div>");
    </c:forEach>
  })
</script>
```
스크립트단에서 jstl `<c:foreach>`로 List를 받아왔고 studyVO로 변수를 받아와 각각의 속성들을 사용할 수 있었다.

**2017-11-30할일**
1. 등록: 카테고리 구현
2. 등록: 파일업로드 input:file로
3. 리스트: 썸네일 차례로 정렬
