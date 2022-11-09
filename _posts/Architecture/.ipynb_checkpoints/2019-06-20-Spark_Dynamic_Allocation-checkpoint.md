---
layout: post
title: '[Spark] Spark Dynamic Allocation'
category: Architecture
tags: [hadoop, spark]
comments: true
---

# Environment
- CentOS 7
- Hadoop 2.7.3
- Spark 2.4.0


# Spark Dynamic Allocation

## yarn-site.xml

~~~html
<!-- yarn-site.xml -->

<property>
  <name>yarn.resourcemanager.work-preserving-recovery.scheduling-wait-ms</name>
  <value>30000</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle,timeline_collector,spark_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.timeline_collector.class</name>
  <value>org.apache.hadoop.yarn.server.timelineservice.collector.PerNodeTimelineCollectorsAuxService</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.spark_shuffle.class</name>
  <value>org.apache.spark.network.yarn.YarnShuffleService</value>
</property>

<property>
<name>yarn.nodemanager.resource.cpu-vcores</name>
  <value>4</value>
</property>
<property>
<name>yarn.nodemanager.pmem-check-enabled</name>
  <value>false</value>
</property>
~~~

## spark-default.conf

~~~sh
# spark-default.conf

spark.shuffle.service.enabled   true
spark.dynamicAllocation.enabled true
spark.dynamicAllocation.schedulerBacklogTimeout 3s
spark.dynamicAllocation.sustainedSchedulerBacklogTimeout 3s
~~~