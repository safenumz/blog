---
layout: post
title: '[Jupyter Notebook] env'
category: DevOps
tags: [jupyter notebook, env]
comments: true
---

# env
env는 anaconda 내부의 독립적인 환경이다.

## env 생성

~~~shell
- py35라는 이름으로 파이썬 3.5 버전 환경 설정
$ conda create -n py35 python=3.5
~~~

## env 삭제

~~~shell
$ conda env remove -n tutorial
~~~

## env 목록 보기

~~~shell
$ conda env list
$ conda info --envs
~~~

## env 활성화

~~~shell
$ conda activate py35
~~~

## env 비활성화

~~~shell
$ deactivate
~~~


# Virtual env

## 1. 가상 환경 활성화

~~~shell
$ source activate [virtualEnv]
~~~

## 2. 가상환경에서 Jupyter Notebook 설치

~~~shell
$ pip install ipykernel
~~~

## 3. Jupyter Notebook에 가상환경 kernel 추가

~~~shell
$ python -m ipykernel install --user --name [virtualEnv] --display-name "[displayKernelName]
~~~

또는

~~~shell
$ ipython kernel install --user --name [virtualEnv] --display-name "[displayKernelName]"
~~~

> 만약 위 방식으로 커널이 생성되지 않는다면 직접 추가한다.
> ~/.local/share/jupyter/kernles/kernel.json 파일을 편집기를 통해 수정한다.


## Jupyter kernel 삭제

~~~shell
$ jupyter kernel uninstall [kernelName]
~~~

# 실행 예시

~~~shell
# Add the conda-forge channel
$ conda config --add channels conda-forge

# Create an isolated environment called `java_env` and install the kernel
$ conda create --name java_env

# Activate the `java_env` environment
$ source activate java_env

# Check the kernel has been installed
$ jupyter kernelspec list
~~~

~~~shell
$ conda --version

# 최신버전으로 conda update
$ conda update conda

# 환경설정 생성과 활성화
$ conda create --name snowflackes biopython

$ conda activate snowflakes

$ conda create --name bunnies python=3 astroid babel

# 모든 환경설정 목록
$ conda info --envs

# 환경설정 복사본 만들기
$ conda create --name flowers --clone snowflakes

# 복사본이 제대로 만들어졌는지 확인
$ conda info --envs

# 환경설정 지우기
$ conda remove --name flowers -all

# 제대로 지워졌는지 확인
$ conda info --envs

# 추가 명령어 확인
$ conda remove --help

# View a list of packages and versions installed in an environment
$ conda list

# Search for a package
# conda search beautifulsoup4

# Install a new packages
# conda install --name bunnies beautifulsoup4

# Remove a package
$ conda remove --name bunies iopro

# Confirm that program has been removed
$ conda list

# Remove an environment
$ conda remove --name snake --all

# Verify environment was removed
$ conda info --envs
~~~

