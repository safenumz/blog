---
layout: post
title: '[Jupyter Notebook] C 코딩하기'
category: DevOps
tags: [jupyter notebook, c]
comments: true
---

# jupyter notebook에서 C 코딩하기
Linux와 OS X에서만 작동한다. 안타깝게도 Windows에서는 작동하지 않는다. gcc, jupyter, python3, pip 가 설치되어 있어야 한다.

```shell
$ pip install jupyter-c-kernel

$ install_c_kernel

$ jupyter-notebook

# 기존 gcc에서 에러가 발생한다면 아나콘다로 gcc 설치
$ conda install -c anaconda mingw
```
