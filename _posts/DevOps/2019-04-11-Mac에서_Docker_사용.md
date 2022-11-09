---
layout: post
title: '[Docker] MacOS에서 Docker 사용'
category: DevOps
tags: [docker, mac]
comments: true
---

## Docker 클라이언트 설치

```shell
$ brew install Docker
```

```shell
$ docker version
```

<pre>
Client: Docker Engine - Community
 Version:           18.09.4
 API version:       1.39
 Go version:        go1.12.1
 Git commit:        d14af54
 Built:             Fri Mar 29 03:27:05 2019
 OS/Arch:           darwin/amd64
 Experimental:      false
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
</pre>




## Docker Application 설치

도커 프로그램을 다운받아 Applications에 넣어준다.

[https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/)

```shell
$ docker version
```

<pre>
Client: Docker Engine - Community
 Version:           18.09.2
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        6247962
 Built:             Sun Feb 10 04:12:39 2019
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.2
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.6
  Git commit:       6247962
  Built:            Sun Feb 10 04:13:06 2019
  OS/Arch:          linux/amd64
  Experimental:     false
</pre>

