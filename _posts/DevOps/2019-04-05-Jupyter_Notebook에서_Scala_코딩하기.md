---
layout: post
title: '[Jupyter Notebook] Scala 코딩하기'
category: DevOps
tags: [jupyter notebook, scala]
comiments: true
---

# [almond](https://almond.sh/docs/quick-start-install)


```shell
$ SCALA_VERSION=2.12.8 ALMOND_VERSION=0.4.0

$ curl -Lo coursier https://git.io/coursier-cli
$ chmod +x coursier
$ ./coursier bootstrap \
    -r jitpack \
    -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
    sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
    -o almond

$ ./almond --install
```
