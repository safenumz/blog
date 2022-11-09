---
layout: post
title: '[Jupyter] Docker 기반 딥러닝 Jupyter Notebook 및 VSCode Server 구축'
category: DevOps
tags: [jupyter, notebook, docker, vscode, nvidia, tensorflow]
comments: true
---

# Environment
- CentOS 7
- nvidia-smi 510.60.02
- Docker 20.10.1
- docker-compose 1.27.4

# Docker 기반 딥러닝 Jupyter Notebook 및 VSCode Server 구축

## 실행
```sh
# 시작
$ docker-compose up -d
# 중단
$ docker-compose down
```

## 구성 파일
- .env
- docker-compose.yml
- dev.Dockerfile
- tensorflow.Dockerfile

### .env

```sh
HOSTNAME=`hostname`
ROOT_DIR=/lab # path on the host

VSCODE_PORT=<VSCODE_PORT>
VSCODE_PASSWORD=<VSCODE_PASSWORD>
VSCODE_DAT_DIR=/lab/devtools/dev/vscode/data/pkgs

JUPYTER_PORT=<JUPYTER_PORT>
JUPYTER_PASSWORD=<JUPYTER_PASSWORD>
JUPYTER_DIR=/lab         # path in the container

TENSORBOARD_PORT=<TENSORBOARD_PORT>
TENSORBOARD_DIR=/lab/devtools/dev/tensorboard/data/runs         # path in the container

WORKING_DIR=/lab
```

### docker-compose.yml

```sh
version: '3.8'

services:
  tensorboard:
    image: tensorboard
    build:
      context: .
      dockerfile: tensorboard.Dockerfile
    restart: always
    container_name: tensorboard
    hostname: ${HOSTNAME}-tensorboard
    ports:
      - ${TENSORBOARD_PORT}:${TENSORBOARD_PORT}
    volumes:
      - ${ROOT_DIR}:/jupyter
    command:
      [
        "tensorboard",
        "--logdir=${TENSORBOARD_DIR}",
        "--port=${TENSORBOARD_PORT}",
        "--bind_all",
      ]
  dev:
    image: dev
    build:
      context: .
      dockerfile: dev.Dockerfile
      args:
        - VSCODE_PASSWORD=${VSCODE_PASSWORD}
        - JUPYTER_PASSWORD=${JUPYTER_PASSWORD}
    restart: always
    container_name: dev
    hostname: ${HOSTNAME}-dev
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=0
      - CUDA_VISIBLE_DEVICES=0
    volumes:
      - ${ROOT_DIR}:${ROOT_DIR}
    ports:
      - ${JUPYTER_PORT}:${JUPYTER_PORT}
      - ${VSCODE_PORT}:${VSCODE_PORT}
    privileged: true
    stdin_open: true
    tty: true
    working_dir: ${WORKING_DIR}
    command:
      [
       "/bin/bash",
       "/start.sh",
       "${VSCODE_PORT}",
       "${VSCODE_DAT_DIR}",
       "${JUPYTER_PORT}",
      ]
```

### dev.Dockerfile

```sh
FROM nvidia/cuda:11.0-cudnn8-runtime-ubuntu18.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Seoul

# ensure system is updated and has basic build tools
RUN apt-get update -y && apt-get install -y build-essential \
    tzdata git \
    curl wget lftp \
    openssh-server openssh-client sshpass \
    zip unzip pigz \
    vim htop \
        python3-dev python3-pip python3-setuptools \
    default-jdk default-jre \
    locales

RUN sed -i 's/^# \(ko_KR.UTF-8\)/\1/' /etc/locale.gen
RUN locale-gen
RUN localedef -f UTF-8 -i ko_KR ko_KR.UTF-8

ENV LC_ALL=ko_KR.UTF-8
ENV LANGUAGE=ko

# Nodejs
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && apt-get install -y nodejs && apt-get clean && rm -rf /var/lib/apt/lists/*

# Google Chrome Driver
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' && apt-get update -y && apt-get install google-chrome-stable -y

# DataScience
RUN python3 -m pip install --upgrade pip && pip3 install numpy pandas scipy scikit-learn scikit-image pyarrow

# Visualization
RUN pip3 install matplotlib seaborn plotly

# Crawling
RUN pip3 install urllib3 beautifulsoup4 fake_useragent webdriver_manager selenium

# DB
RUN pip3 install sqlalchemy elasticsearch PyMySql lmdb

# DL
RUN pip3 install Keras tensorflow-gpu==2.4.0 torch torchvision

# NLP
RUN pip3 install nltk transformers contractions kobert_transformers

# Computer Vision
RUN pip3 install opencv-python-headless shapely

# ETC
RUN pip3 install tendo paramiko scp natsort tqdm minio

# Notebook
RUN pip3 install jupyter jupyterlab ipykernel && python3 -m ipykernel.kernelspec

# Vscode-server
RUN curl -fsSL https://code-server.dev/install.sh | sh

ARG VSCODE_PASSWORD
ARG JUPYTER_PASSWORD
ENV PASSWORD=$VSCODE_PASSWORD
ENV JUPYTER_PASSWORD=$JUPYTER_PASSWORD
COPY start.sh /start.sh

WORKDIR /
```

### tensorboard.Dockerfile

```sh
FROM python:3.8-slim-buster
RUN pip install tensorboard
```