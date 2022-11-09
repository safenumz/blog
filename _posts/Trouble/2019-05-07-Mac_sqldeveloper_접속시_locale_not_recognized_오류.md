---
post: layout
title: '[Trouble sqldeveloper] Mac에서 접속 시 locale not recognized 에러'
category: Trouble
tags: [sql, locale not recognized]
comments: true
---

# Environment
- MacOS Mojave 10.14.5

# Trouble
- Mac에서 sqldeveloper 접속시 locale not recognized 에러가 발생하는 경우

# Shooting
- sqldeveloper 우클릭 > 패키지 내용보기 선택 후에
- Contents/Resources/sqldeveloper/bin/sqldeveloper.conf 파일을 열어서 다음과 같은 옵션을 추가 한다. 지정된 경로에 sqldeveloper.conf 파일이 없으면 새로 생성하면 된다.

~~~shell
AddVMOption -Duser.language=ko

AddVMOption -Duser.country=KR 
~~~

- 저장 후 sqldeveloper를 실행하면 로케일이 KR로 바뀐다.