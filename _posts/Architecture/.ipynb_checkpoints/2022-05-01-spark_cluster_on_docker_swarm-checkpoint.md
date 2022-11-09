---
layout: post
title: '[Spark] Docker Swarm에서 Spark Cluster, Zeppelin 구축하기'
category: Architecture
tags: [spark, docker]
comments: true
---

## CentOS 7 사용중인 포트 확인

```sh
$ netstat -tulpn | grep LISTEN
$ firewall-cmd --zone=public --list-ports
```

## Docker Swarm을 위한 Port Open
- TCP port 2376 for secure Docker client communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts.
- TCP port 2377 for cluster management communications. It only needs to be opened on manager nodes.
- TCP and UDP port 7946 for communication among nodes
- UDP port 4789 for overlay network traffic

```sh
$ firewall-cmd --add-port=2376/tcp --permanent
$ firewall-cmd --add-port=2377/tcp --permanent
$ firewall-cmd --add-port=7946/tcp --permanent
$ firewall-cmd --add-port=7946/udp --permanent
$ firewall-cmd --add-port=4789/udp --permanent
$ firewall-cmd --reload
```

## Spark를 위한 Port Open

```sh
$ firewall-cmd --add-port=4040/tcp --permanent
$ firewall-cmd --add-port=6066/tcp --permanent
$ firewall-cmd --add-port=7077/tcp --permanent
$ firewall-cmd --add-port=8080/tcp --permanent
$ firewall-cmd --add-port=8081/tcp --permanent
$ firewall-cmd --add-port=19999/tcp --permanent
$ firewall-cmd --reload
```

## Manager: Docker Swarm 시작

```sh
# docker swarm init --advertise-addr 192.168.0.100
$ docker swarm init --advertise-addr <MANAGER_NODE_IP>

# docker swarm join token 얻기
$ docker swarm join-token manager
```
docker swarm join --token SWMTKN-1-3k7lnqqrvak9o7tc1keeslu6jxzkkmibpwbbx7a6n6j95wldd7-1e0bf81z39dbzbjgvvalebryg 192.168.0.100:2377

## Worker: Master Node에 연결

```sh
# docker swarm join --token SWMTKN-1-3k7lnqqrvak9o7tc1keeslu6jxzkkmibpwbbx7a6n6j95wldd7-1e0bf81z39dbzbjgvvalebryg 192.168.0.100:2377
$ docker swarm join --token <SWARM_TOKEN> <MANAGER_NODE_IP>:2377
```

## Master, Worker: sshfs plugin 설치
- Docker Swarm의 Persistant Volume으로 sshfs 사용, 다른거 써도 됨

```sh
# docker plugin install --grant-all-permissions vieux/sshfs
$ docker plugin install --grant-all-permissions vieux/sshfs DEBUG=1 sshkey.source=/home/a/.ssh/

$ docker plugin ls
```

## Master: docker swarm 용 docker-compose 작성
- 편의상 bitnami/spark:3.1.2 을 사용했는데, 권한이 너무 제한적이라 편집이 쉽지 않음, 차라리 처음부터 직접 만드는 게 낫다고 생각되지만 여기선 그냥 진행함
- zeppelin을 사용하기 위해 docker-compose.yml의 하위 경로에 기존 zeppelin에서 사용하던 zeppelin config 파일들을 상황에 맞게 수정하여 넣어둠
    - ./zeppelin/config/zeppelin-env.sh
    - ./zeppelin/config/zeppelin-site.xml
    - ./zeppelin/config/shiro.ini
    
### zeppelin.Dockerfile

```sh
FROM bitnami/spark:3.2.1
COPY zeppelin-0.10.1-bin-all.tgz /tmp/zeppelin.tgz
RUN tar -xzvf /tmp/zeppelin.tgz -C /opt/ \
    && mv /opt/zeppelin-* /opt/zeppelin

# ENV SPARK_SUBMIT_OPTIONS "--jars /opt/zeppelin/sansa-examples-spark-2016-12.jar"

WORKDIR /opt/zeppelin

CMD ["/opt/zeppelin/bin/zeppelin.sh"]
```

### zeppelin.Dockerfile build & push

```sh
$ docker build -f zeppelin.Dockerfile -t safenumz/spark-zeppelin:0.1

$ docker push safenumz/spark-zeppelin:0.1
```

### docker-compose.yml

```sh
version: '3.8'

services:
  spark-master:
    deploy:
      placement:
        constraints: [node.hostname == lab101]
    image: bitnami/spark:3.1.2
    user: 0:0
    command: bin/spark-class org.apache.spark.deploy.master.Master
    hostname: spark-master
    environment:
      SPARK_MODE: master
      MASTER: spark://spark-master:7077
    networks:
      - spark
    ports:
      - 4040:4040
      - 6066:6066
      - 7077:7077
      - 8080:8080
    volumes:
      - type: volume
        source: ssh-server
        target: /opt/zeppelin/notebook/persistent

  spark-worker:
    deploy:
      placement:
        constraints: [node.labels.worker != false]
      replicas: 3
    image: bitnami/spark:3.1.2
    command: bin/spark-class org.apache.spark.deploy.worker.Worker spark://spark-master:7077
    hostname: spark-worker
    environment:
      SPARK_MODE: worker
      SPARK_WORKER_CORES: 4
      SPARK_WORKER_MEMORY: 13g
    networks:
      - spark
    ports:
      - 8081:8081

  zeppelin:
    deploy:
      placement:
        constraints: [node.hostname == lab101]
    image: safenumz/spark-zeppelin:0.1
    hostname: zeppelin
    user: 0:0
    configs:
      - source: zeppelin_xml
        target: /opt/zeppelin/conf/zeppelin-site.xml
        mode: 444
      - source: zeppelin_shiro
        target: /opt/zeppelin/conf/shiro.ini
        mode: 444
      - source: zeppelin_env
        target: /opt/zeppelin/conf/zeppelin-env.sh
        mode: 444
    environment:
      - "MASTER=spark://spark-master:7077"
      - "SPARK_MASTER=spark://spark-master:7077"
    networks:
      - spark
    ports:
      - 19999:8888
    volumes:
      - type: volume
        source: ssh-server
        target: /opt/zeppelin/notebook/persistent

configs:
  zeppelin_env:
    file: ./zeppelin/config/zeppelin-env.sh
  zeppelin_xml:
    file: ./zeppelin/config/zeppelin-site.xml
  zeppelin_shiro:
    file: ./zeppelin/config/shiro.ini

networks:
  spark:
    attachable: true

volumes:
  ssh-server:
    driver: vieux/sshfs
    driver_opts:
      sshcmd: "a@192.168.0.101:/lab/storages/private/zeppelin/notebook/persistent"
```

## Master: Spark Cluster 실행

```sh
# 시작
$ docker stack deploy --compose-file=docker-compose.yml spark

# 종료
$ docker stack rm spark
```

```sh
$ docker node ls
$ docker service ls
```



