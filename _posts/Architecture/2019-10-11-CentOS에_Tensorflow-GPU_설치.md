---
layout: post
title: '[Cuda] CentOS에 tensorflow-gpu 1.9.0 버전 설치'
category: Architecture
tags: [centos, tensorflow-gpu]
comments: true
---

# Environment
- CentOS 7
- Anaconda3-5.2.0-Linux-x86_64.sh
- Python 3.6
- NVIDIA GTX 1060
- NVIDIA-SMI 390.59
- Cuda 9.0

# GPU Env Dependency

NVIDIA GPU      | NVIDIA-SMI version | Cuda version | tensorflow-gpu version 
:---------------|:-------------------|:------------|:-----------------
NVIDIA GTX 1060 | NVIDIA-SMI 390.59  | Cuda 9       | tensorflow-gpu 1.9.0
<br>            | NVIDIA-SMI 415     | Cuda 10      | tensorflow-gpu 2.0.0 


# tensorflow-gpu 설치
- NVIDIA-SMI 390.59 기준

~~~shell
$ conda install -y cudnn cupti cudatoolkit==9.0

$ pip install tensorflow-gpu==1.9.0
~~~

# tensorflow-gpu 구동하는지 확인

~~~python
>>> import tensorflow as tf

>>> tf.__version
1.9.0

>>> tf.test.is_gpu_available()
TRUE
~~~