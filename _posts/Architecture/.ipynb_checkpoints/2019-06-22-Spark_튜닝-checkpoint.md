---
layout: post
title: '[Spark] Spark 튜닝'
category: Architecture
tags: [spark, yarn, hadoop]
comments: true
---

# Environment
- r4.xlarge (4 CPU / 30GB 메모리) * 1 - Cloudera Manager용 서버
- r4.4xlarge (16 CPU / 120GB 메모리) * 3 - data node용 호스트 서버

# 1. YARN configuration 설정
- yarn.nodemanager.resource.memory-mb : 서버에서 컨테이너에 할당할 수 있는 물리적 메모리 총량을 의미합니다. 서버(120GB)에서 시스템 프로세스를 위한 여유분(1GB)을 남겨두고 119GB의 용량으로 설정합니다.
- yarn.nodemanager.resource.cpu-vcores : 컨테이너에 할당할 수 있는 가상 코어 수입니다. 이는 Spark executor에서의 task, 즉 이용하는 core의 수와 의미하는 바가 같습니다. 한 서버에서 사용할 수 있는 코어는 16개의 CPU중 시스템을 위한 한 개의 CPU를 제외하고 15로 설정합니다.
- yarn.scheduler.maximum-allocation-mb : 한 컨테이너에 최대로 요청할 수 있는 메모리 값을 의미합니다. 119GB의 용량을 앞에서 할당해 주었으므로 119GB로 설정합니다.
- yarn.scheduler.minimum-allocation-mb : 한 컨테이너에서 요청할 수 있는 메모리의 최솟값, 최소 단위입니다. default값인 1GB로 설정하여 둡니다.


# 2. Hive configuration 설정
- spark.executor.cores = number of CPUs on a worker node
- spark.executor.instances = number of worker nodes on a cluster
- spark.executor.memory = max memory available on a worker node - overheads
- spark.default.parallelism = 2 * number of CPUs in total on worker nodes

<br>

- spark.executor.cores : 한 Spark 실행기에서 사용하는 코어의 수입니다. 많은 수의 코어를 사용하면 여러 개의 task에서 하나의 JVM에 데이터를 공유할 수 있도록 만들어줍니다. 하지만 너무 많은 수의 코어가 있을 경우 HDFS I/O의 성능이 떨어져 속도가 저하되는 문제가 발생합니다. 통상적으로 하나의 executor당 5개의 코어를 사용할 때 효율이 가장 좋다고 합니다. 값은 5로 설정하여 줍니다.
- spark.executor.instances : 애플리케이션 당 실행할 수 있는 실행기의 수를 의미합니다. 우리는 16 CPU를 가진 3대의 서버를 이용하므로 48개의 코어가 존재하고, 각각이 시스템(3개의 r4.4xlarge worker server)을 위한 한 개의 코어를 사용한다고 가정할 때 사용할 수 있는 코어는 45개입니다. 하나의 executor당 5개의 코어(spark.executor.cores)를 사용하므로 애플리케이션 당 실행할 수 있는 실행기는 45/5=9로 설정하여 줍니다.
- spark.executor.memory, spark.yarn.executor.memoryOverhead : Spark 실행기의 JAVA 최대 메모리 힙 크기를 의미합니다. 앞에서 우리는 총 9개의 실행기, 한 서버에서 평균적으로 3개의 실행기를 실행합니다. 앞에서 우리가 사용하는 메모리의 크기는 119GB, 3개의 실행기를 사용하기 위해서는 하나의 실행기 당 119/3으로 대략 39GB의 메모리를 할당하여 줍니다. 이 때, 할당된 메모리의 0.07%가 메모리 오버헤드를 위한 메모리 영역으로 할당된다고 하여, 39*0.07=2.8GB가 memoryOverhead로 잡히게 되고, 남은 약 36GB가 executor.memory로 할당되도록 합니다.
