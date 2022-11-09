---
post: layout
title: '[Trouble mysql] COM.MYSQL.JDBC.DRIVER 에러'
category: Trouble
tags: [mysql, com.mysql.jdbc.driver]
comments: true
---

# Environment
- MacOS Mojave 10.14.5

# Trouble
- MySQL의 COM.MYSQL.JDBC.DRIVER 에러

<pre>
 LOADING CLASS `COM.MYSQL.JDBC.DRIVER’. THIS IS DEPRECATED. THE NEW DRIVER CLASS IS `COM.MYSQL.CJ.JDBC.DRIVER’. THE DRIVER IS AUTOMATICALLY REGISTERED VIA THE SPI AND MANUAL LOADING OF THE DRIVER CLASS IS GENERALLY UNNECESSARY.
</pre>

# Shooting
- driver 이름 변경 필요
- 변경 전: com.mysql.jdbc.Driver
- 변경 후: com.mysql.cj.jdbc.Driver