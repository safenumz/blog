---
layout: post
title: '[Trouble spark-shell] java.lang.ExceptionInInitializerError 에러'
category: Trouble
tags: [trouble, spark, spark-shell, java.lang.ExceptionInInitializerError]
commet: true
---

# Environment
- Windows 10
- Spark 2.4.0
- Java 12

# Trouble
- spark-shell을 실행했을 때, java.lang.ExceptionInInitializerError 오류가 뜨면서 실행이 안되는 경우가 있다. 이는 자바를 최신 12버전으로 설치했을 때 발생하는 오류이다. Java SE 8버전 또는 Java SE 11버전을 설치하여 해결한다.

```sh
$ ./spark-shell.cmd
```

<pre>
<meta charset="utf-8"/>
Exception in thread "main" java.lang.ExceptionInInitializerError
        at org.apache.hadoop.util.StringUtils.&lt;clinit&gt;(StringUtils.java:80)
        at org.apache.hadoop.security.SecurityUtil.getAuthenticationMethod(SecurityUtil.java:611)
        at org.apache.hadoop.security.UserGroupInformation.initialize(UserGroupInformation.java:273)
        at org.apache.hadoop.security.UserGroupInformation.ensureInitialized(UserGroupInformation.java:261)
        at org.apache.hadoop.security.UserGroupInformation.loginUserFromSubject(UserGroupInformation.java:791)
        at org.apache.hadoop.security.UserGroupInformation.getLoginUser(UserGroupInformation.java:761)
        at org.apache.hadoop.security.UserGroupInformation.getCurrentUser(UserGroupInformation.java:634)
        at org.apache.spark.util.Utils$$anonfun$getCurrentUserName$1.apply(Utils.scala:2422)
        at org.apache.spark.util.Utils$$anonfun$getCurrentUserName$1.apply(Utils.scala:2422)
        at scala.Option.getOrElse(Option.scala:121)
        at org.apache.spark.util.Utils$.getCurrentUserName(Utils.scala:2422)
        at org.apache.spark.SecurityManager.&lt;init&gt;(SecurityManager.scala:79)
        at org.apache.spark.deploy.SparkSubmit.secMgr$lzycompute$1(SparkSubmit.scala:359)
        at org.apache.spark.deploy.SparkSubmit.org$apache$spark$deploy$SparkSubmit$$secMgr$1(SparkSubmit.scala:359)
        at org.apache.spark.deploy.SparkSubmit$$anonfun$prepareSubmitEnvironment$7.apply(SparkSubmit.scala:367)
        at org.apache.spark.deploy.SparkSubmit$$anonfun$prepareSubmitEnvironment$7.apply(SparkSubmit.scala:367)
        at scala.Option.map(Option.scala:146)
        at org.apache.spark.deploy.SparkSubmit.prepareSubmitEnvironment(SparkSubmit.scala:366)
        at org.apache.spark.deploy.SparkSubmit.submit(SparkSubmit.scala:143)
        at org.apache.spark.deploy.SparkSubmit.doSubmit(SparkSubmit.scala:86)
        at org.apache.spark.deploy.SparkSubmit$$anon$2.doSubmit(SparkSubmit.scala:924)
        at org.apache.spark.deploy.SparkSubmit$.main(SparkSubmit.scala:933)
        at org.apache.spark.deploy.SparkSubmit.main(SparkSubmit.scala)
Caused by: java.lang.StringIndexOutOfBoundsException: begin 0, end 3, length 2
        at java.base/java.lang.String.checkBoundsBeginEnd(String.java:3410)
        at java.base/java.lang.String.substring(String.java:1883)
        at org.apache.hadoop.util.Shell.&lt;clinit&gt;(Shell.java:52)
        ... 23 more
</pre>

# Shooting
## Java SE 8버전 설치

## Java SE 11버전 설치
[https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)


