---
layout: post
title: '[Trouble git] LF will be replaced by CRLF 에러'
category: Trouble
tags: [trouble, git, LF will be replaced by CRLF]
comments: true
---

# Enviroment
- Windows 10

# Trouble
- Git에서 'git add .' 명령어 실행 시 warning: LF will be replaced by CRLF 에러가 발생하는 경우

# Shooting
- 문서의 끝을 처리하는데 있어서 OS마다 약간의 차이가 있기 때문에 발생
- core.autocrlf 기능 꺼주기

~~~sh
$ git config --global core.autocrlf false
~~~