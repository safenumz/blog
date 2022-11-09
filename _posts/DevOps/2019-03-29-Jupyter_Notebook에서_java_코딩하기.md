---
layout: post
title: '[Jupyter Notebook] Java 코딩하기'
category: DevOps
tags: [jupyter notebook, java]
comments: true
---

# jupyter notebook에서 java 코딩하기
jupyter notebook에서 java를 코딩하는 방법은 크게 3가지가 있다. Ijava kernel과 scijava-jupyter-kernel, 그리고 자바를 따로 설치하지 않고 웹서버에서 구동되는 ImageJ를 이용하는 것이다. 주의할 점은 Ijava kernel은 자바 9버전 이상이 설치되어 있어야 한다. 본인은 apache spark를 사용하는 관계로 자바 9버전을 설치할 수 없다. 따라서 scijava-jupyter-kernel를 사용하였다.

## 1. Ijava kernel
[https://github.com/SpencerPark/IJava](https://github.com/SpencerPark/IJava)

자바는 9버전 이상 설치되어 있어야 한다. 하지만 Apache Spark를 사용하고 있다면 자바 9버전 이상을 설치 할 수 없을 것이다. 만약 자바 7~8 버전을 사용하고 있다면 아래 두 번째에 소개되어 있는 scijava를 이용하자.

```shell
$ java -version
```

[https://github.com/SpencerPark/IJava/releases](https://github.com/SpencerPark/IJava/releases)

ijava-1.2.0.zip을 임시 폴더에 다운받고 압축을 푼다. 압축을 풀게 되면 install.py 파일과 java 폴더가 있다. install.py 파일을 파이썬3로 실행시킨다.

```shell
# Pass the -h option to see the help page
$ python3 install.py -h
```

java kernel이 생성되었는지 확인한다.

```shell
$ jupyter kernelspec list
```

jupyter notebook을 실행하여 Java kernel로 노트북을 작성하면 된다.

```shell
$ jupyter notebook
```


## 2. scijava-jupyter-kernel
[https://github.com/scijava/scijava-jupyter-kernel](https://github.com/scijava/scijava-jupyter-kernel)
- 아나콘다가 설치되어 있어야 한다.

~~~shell
# Add the conda-forge channel
$ conda config --add channels conda-forge

# Create an isolated environment called 'java_env' and install the kernel
$ conda create --name java_env scijava-jupyter-kernel

# Activate the 'java_env' environment
$ conda activate java_env

# Check the kernel has been installed
$ jupyter kernelspec list

# Launch your favorite Jupyter client
$ jupyter notebook
~~~

## 3. ImageJ
현재 데스크탑에 자바가 설치되있지 않아도 Web에서 자바를 실행할 수 있다. ImageJ를 사용하면 jupyter notebook 형태로 자바를 사용할 수 있다. 자바가 설치되어 있지 않는 환경에서 사용하고 파일로 저장할 수 있다.

[https://mybinder.org/v2/gh/4QuantOSS/scijava-jupyter-kernel/master?filepath=notebooks%2FImageJ.ipynb](https://mybinder.org/v2/gh/4QuantOSS/scijava-jupyter-kernel/master?filepath=notebooks%2FImageJ.ipynb)
