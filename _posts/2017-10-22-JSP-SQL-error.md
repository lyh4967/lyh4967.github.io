---
title: "[JSP] SQL에러 연결종료"
date: 2017-10-22 10:18:00
categories:
- JSP
tags:
- JSP
- ERROR
---
conn을 닫아줄 시에 연결종료가 발생한다. pstmt혹은 rs를 닫고 나서는 다시 정상적으로 선어이 돼지만 conn의 경우는 한번 닫아줄 경우 재연결 시에 예외가 발생한다

예상해 보건데 싱글턴을 사용하면 기존에 있던 객체를 계속 사용하기 때문에 conn 재연결이 안돼 위와 같은 에러가 발생하는 듯 한다. connection pool도 사용해보자.
