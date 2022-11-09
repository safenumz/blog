---
layout: post
title: '[Jupyter Notebook] R 코딩하기'
category: DevOps
tags: [jupyter notebook, r]
comiments: true
---

# IRKernel
- 사전에 아나콘다가 설치되어 있어야 한다. 'conda create -n r-env -c r r-essentials'을 실행하면 r뿐만 아니라 필수패키지들이 함께 설치된다. 사실 이 부분만 진행해도 상관이 없다. 그러나 간혹 r커널이 제대로 셋팅이 안되는 경우가 있어 아래 명령어를 전부 순서대로 실행한다. 

```shell
$ conda create -n r anaconda

$ conda install -c r r

$ conda install -c r r-irkernel

$ conda install -c r r-essentials
```