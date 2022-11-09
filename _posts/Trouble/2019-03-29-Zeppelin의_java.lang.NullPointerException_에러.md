---
layout: post
title: '[Trouble zeppelin] java.lang.NullPointerException 에러'
category: Trouble
tags: [zeppelin, spark, java.lang.NullPointerException]
commet: true
---

# Environment
- Windows 10
- Spark 2.4.0
- Zeppelin 0.8.0
- Python 3.7
- Scala 2.12

# Trouble
- Zeppelin 실행시 java.lang.NullPointerException 에러가 발생하는 경우

# Shooting
- Spark runs on Java 8+, Python 2.7+/3.4+ and R 3.1+. For the Scala API, Spark 2.4.0 uses Scala 2.11. You will need to use a compatible Scala version (2.11.x).
- Note that support for Java 7, Python 2.6 and old Hadoop versions before 2.6.5 were removed as of Spark 2.2.0. Support for Scala 2.10 was removed as of 2.3.0.


## 관련 링크
[https://spark.apache.org/docs/latest/index.html](https://spark.apache.org/docs/latest/index.html)
<br/>
[https://zeppelin.apache.org/supported_interpreters.html](https://zeppelin.apache.org/supported_interpreters.html)
<br/>
[https://www.scala-lang.org/download/2.11.12.html](https://www.scala-lang.org/download/2.11.12.html)


<pre>
java.lang.NullPointerException
	at org.apache.thrift.transport.TSocket.open(TSocket.java:170)
	at org.apache.zeppelin.interpreter.remote.ClientFactory.create(ClientFactory.java:51)
	at org.apache.zeppelin.interpreter.remote.ClientFactory.create(ClientFactory.java:37)
	at org.apache.commons.pool2.BasePooledObjectFactory.makeObject(BasePooledObjectFactory.java:60)
	at org.apache.commons.pool2.impl.GenericObjectPool.create(GenericObjectPool.java:861)
	at org.apache.commons.pool2.impl.GenericObjectPool.borrowObject(GenericObjectPool.java:435)
	at org.apache.commons.pool2.impl.GenericObjectPool.borrowObject(GenericObjectPool.java:363)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreterProcess.getClient(RemoteInterpreterProcess.java:62)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreterProcess.callRemoteFunction(RemoteInterpreterProcess.java:133)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter.internal_create(RemoteInterpreter.java:165)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter.open(RemoteInterpreter.java:132)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter.getFormType(RemoteInterpreter.java:299)
	at org.apache.zeppelin.notebook.Paragraph.jobRun(Paragraph.java:407)
	at org.apache.zeppelin.scheduler.Job.run(Job.java:188)
	at org.apache.zeppelin.scheduler.RemoteScheduler$JobRunner.run(RemoteScheduler.java:307)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)

org.apache.thrift.transport.TTransportException
	at org.apache.thrift.transport.TIOStreamTransport.read(TIOStreamTransport.java:132)
	at org.apache.thrift.transport.TTransport.readAll(TTransport.java:86)
	at org.apache.thrift.protocol.TBinaryProtocol.readStringBody(TBinaryProtocol.java:380)
	at org.apache.thrift.protocol.TBinaryProtocol.readMessageBegin(TBinaryProtocol.java:230)
	at org.apache.thrift.TServiceClient.receiveBase(TServiceClient.java:69)
	at org.apache.zeppelin.interpreter.thrift.RemoteInterpreterService$Client.recv_createInterpreter(RemoteInterpreterService.java:209)
	at org.apache.zeppelin.interpreter.thrift.RemoteInterpreterService$Client.createInterpreter(RemoteInterpreterService.java:192)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter$2.call(RemoteInterpreter.java:169)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter$2.call(RemoteInterpreter.java:165)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreterProcess.callRemoteFunction(RemoteInterpreterProcess.java:135)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter.internal_create(RemoteInterpreter.java:165)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter.open(RemoteInterpreter.java:132)
	at org.apache.zeppelin.interpreter.remote.RemoteInterpreter.getFormType(RemoteInterpreter.java:299)
	at org.apache.zeppelin.notebook.Paragraph.jobRun(Paragraph.java:407)
	at org.apache.zeppelin.scheduler.Job.run(Job.java:188)
	at org.apache.zeppelin.scheduler.RemoteScheduler$JobRunner.run(RemoteScheduler.java:307)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:304)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
</pre>