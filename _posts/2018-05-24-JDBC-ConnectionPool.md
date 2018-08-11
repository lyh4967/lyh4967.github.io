---
title: "[JDBC] 서블릿 커넥션 풀 설정 "
date: 2018-05-24 11:30:00
categories:
- Java
tags:
- JDBC
- SERVLET
---
**JDBC 커넥션 풀을 연결해보자**

```java
package jdbc;

import org.apache.commons.dbcp2.*;
import org.apache.commons.pool2.impl.GenericObjectPool;
import org.apache.commons.pool2.impl.GenericObjectPoolConfig;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import java.sql.DriverManager;

public class DBCPInit extends HttpServlet {

    @Override
    public void init() throws ServletException{
        loadJDBCDriver();
        initConnetcionPool();
    }

    private void loadJDBCDriver(){
        try{
            Class.forName("com.mysql.jdbc.Driver");//드라이버로딩(mysql)
        }catch(ClassNotFoundException ex){
            throw new RuntimeException("fail to JDBC Driver",ex);
        }
    }
    private void initConnetcionPool(){
        try{
            String jdbcUrl="jdbc:mysql://localhost:3306/(schema이름)?"+ //jdbcURL
                    "useUnicode=true&characterEncoding=utf8";
            String dbUser="(DB사용자id)";//DB 아이디
            String dpPass="(DB사용자pw)";//DB 비밀번호

            ConnectionFactory connFactory=//커넥션 팩토리 생성
                    new DriverManagerConnectionFactory(jdbcUrl,dbUser,dpPass);
            PoolableConnectionFactory poolableConnectionFactory=
                    new PoolableConnectionFactory(connFactory,null);
            poolableConnectionFactory.setValidationQuery("select 1");//커넥션 유효여부

            GenericObjectPoolConfig poolConfig=new GenericObjectPoolConfig();
            poolConfig.setTimeBetweenEvictionRunsMillis(1000L*60L*5L);//커넥션 검사주기
            poolConfig.setTestWhileIdle(true);//풀에보관중이 커넥션 유효여부
            poolConfig.setMinIdle(4);//커넥션 최소갯수
            poolConfig.setMaxTotal(50);//커넥션 최대갯수

            GenericObjectPool<PoolableConnection> connectionPool=
                    new GenericObjectPool<>(poolableConnectionFactory,poolConfig);
            poolableConnectionFactory.setPool(connectionPool);

            Class.forName("org.apache.commons.dbcp2.PoolingDriver");
            PoolingDriver driver=(PoolingDriver)DriverManager.getDriver("jdbc:apache:commons:dbcp:");
            driver.registerPool("(풀이름)",connectionPool);
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
}
```
코드가 다소 복잡하지만 요약하면 이렇다
1. 실제 커넥션을 생성할 ConnectionFactory를 생성.
2. 커넥션 풀로 사용할 PoolableConnection을 생성하는 PoolableConnectionFactory를 생성.
3. 커넥션 풀의 설정 정보를 생성
4. 커넥션 풀을 사용할 JDBC 드라이버를 등록

**jar: commons-dbcp2.jar, commons-pool2.jar, commons-logging.jar**
각 jar파일의 버전은 각각에 맞게 사용하도록 한다.


web.xml
```xml
<servlet>
        <servlet-name>DBCPInit</servlet-name>
        <servlet-class>jdbc.DBCPInit</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
```
마지막으로 web.xml에 매핑을 해준다

이제는 커넥션을 불러올시 다음과 같이 적어준다
```java
String jdbcDriver="jdbc:apache:commons:dbcp:chap14";
```
