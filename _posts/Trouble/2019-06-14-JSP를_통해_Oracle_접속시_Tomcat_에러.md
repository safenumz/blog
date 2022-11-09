---
layout: post
title: '[Trouble tomcat] JSP를 통해 Oracle에 접속시 Tomcat 에러'
category: Trouble
tags: [tomcat]
comments: true
---

# Environment
- MacOS Mojave 10.14.5
- Intelli
- Tomcat 9.0.20

# Trouble
- Intellij에서 JSP를 통해 Oracle에 접속시 Tomcat 관련한 에러가 뜰 때

# Shooting
- Tomcat이 설치된 경로 lib에 오라클 jdbc 관련 jar파일 ojdbc6.jar를 넣어준다.
- MacOS 기준 Tomcat 설치경로 예시> /usr/local/apache-tomcat-9.0.20/lib

~~~shell
$ sudo cp ojdbc6.jar /usr/local/apache-tomcat-9.0.20/lib/ojdbc6.jar
~~~
