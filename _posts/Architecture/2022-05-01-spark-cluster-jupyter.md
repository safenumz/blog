---
layout: post
title: '[Spark] PySpark Cluster 구축 및 Jupyter lab 세팅'
category: Architecture
tags: [spark, jupyter]
comments: true
---

# Environment

| Role | IP | HostName | OS | Ram | CoreNum |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Client | 192.168.0.99 | lab | MacOS | 16G | 12 | 
| Master | 192.168.0.100 | lab100 | CentOS 7 | 16G | 8 |
| Worker | 192.168.0.101 | lab101 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.102 | lab102 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.103 | lab103 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.104 | lab104 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.105 | lab105 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.106 | lab106 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.107 | lab107 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.108 | lab106 | CentOS 7 | 32G | 8 |
| Worker | 192.168.0.109 | lab107 | CentOS 7 | 32G | 8 |

## Client, Master, Woker: hosts 설정
- 지금 다시 생각해 보니까 Client를 MacOS로 하는 건 jars 들을 불러올 때 좋지 않음. 각종 버그가 발생할 수 있음. 그냥 동일한 리눅스로 맞춰주는 게 가장 무난.

```sh
$ vi /etc/hosts
```

```sh
# /etc/hosts

192.168.0.99   lab
192.168.0.100   lab100
192.168.0.101   lab101
192.168.0.102   lab102
192.168.0.103   lab103
192.168.0.104   lab104
192.168.0.105   lab105
192.168.0.106   lab106
192.168.0.107   lab107
192.168.0.108   lab108
192.168.0.109   lab109
```

## Client, Master, Woker: 모든 서버가 ssh 패스워드 없이 접속가능해야 함
- 모든 서버에 ssh public key를 공유하는 건 매우 번거로운 일이라 간단한 스크립트 작성하여 실행함

### 스크립트 실행 전 주의 사항
- CLIENT SERVER에서 실행
- 개인 테스트용으로 만든 스크립트로 운영용이 아님, 운영용으로 만들기 위해서는 더 옵션들을 추가할 필요
- 전 서버에서 sudo 없이 yum command 사용 가능하게 설정해 놓거나, 아니면 사전에 전 서버에 sshpass를 설치해야 함 **yum install sshpass -y**
- 전 서버의 ~/.ssh/authorized_keys 파일에 대해 쓰기 권한이 있어야 함 (일반적으로 400으로 권한 설정해 놓는데 일시적으로 777로 풀어줘야 함)
- 공개키를 새로 생성해서 overwrite하는 방식이라 스크립트를 돌리면 기존 public key는 사용 못하게 됨, 추후 수정 필요

### 스크립트 실행

```sh
$ bash setup_ssh.sh $PWD/ssh_config.sh
```

### ssh_config.sh
- 서버 정보 설정

```sh
#!/bin/bash

# CLIENT=(user  ip  password  port nickname)
# Available OS: CentOS, Ubuntu, Mac
# CLIENT=(a 192.168.0.99 <password> 22 lab)

# SERVER=(user  ip  password  port nickname)
# Available OS: CentOS, Ubuntu
SERVER_0=(a 192.168.0.99 <password> 22 lab)
SERVER_1=(a 192.168.0.100 <password> 22 lab100)
SERVER_2=(a 192.168.0.101 <password> 22 lab101)
SERVER_3=(a 192.168.0.102 <password> 22 lab102)
SERVER_4=(a 192.168.0.103 <password> 22 lab103)
SERVER_5=(a 192.168.0.104 <password> 22 lab104)
SERVER_6=(a 192.168.0.105 <password> 22 lab105)
SERVER_7=(a 192.168.0.106 <password> 22 lab106)
SERVER_8=(a 192.168.0.107 <password> 22 lab107)
SERVER_9=(a 192.168.0.108 <password> 22 lab108)
SERVER_10=(a 192.168.0.109 <password> 22 lab109)

SERVER_LIST=(
    SERVER_0[@]
    SERVER_1[@]
    SERVER_2[@]
    SERVER_3[@]
    SERVER_4[@]
    SERVER_5[@]
    SERVER_6[@]
    SERVER_7[@]
    SERVER_8[@]
    SERVER_9[@]
    SERVER_10[@]
)
```

### setup_ssh.sh
- ssh_config.sh의 서버 정보를 불러들어와서 전 서버를 돌면서 public key를 서로 공유함

```sh
#!/bin/bash
# ===========================================================================
# Program: setup_ssh.sh
#    Path: .
#   Usage: bash setup_ssh.sh $1
#      $1: $PWD/ssh_config.sh
# ============================================================================

source $1

function remote_cmd() {
    local user=$1
    local ip=$2
    local passwd=$3
    local port=$4
    local command=$5
    sshpass -p ${passwd} ssh -p ${port} -o StrictHostKeyChecking=no ${user}@${ip} "export PATH=$PATH:/usr/local/bin:/usr/bin && ${command}"
}

function install_sshpass() {
    local user=$1
    local ip=$2
    local passwd=$3
    local port=$4

    echo "install-sshpass ${ip}:${port}"

    if [[ $(remote_cmd "$user" "$ip" "$passwd" "$port" "cat /etc/*release") =~ "CentOS" ]]; then
        remote_cmd "$user" "$ip" "$passwd" "$port" "yum install -y sshpass"
    elif [[ $(remote_cmd "$user" "$ip" "$passwd" "$port" "cat /etc/*release") =~ "Ubuntu" ]]; then
        remote_cmd "$user" "$ip" "$passwd" "$port" "apt-get install -y sshpass"
    else
        echo "[WARNING] Invalid OS... (Valid OS: CentOS, Ubuntu)"
    fi
}

function ssh_keygen() {
    local user=$1
    local ip=$2
    local passwd=$3
    local port=$4
    local nickname=$5

    echo "*** ssh-keygen ${nickname}(${user}@${ip}:${port})"

    remote_cmd "$user" "$ip" "$passwd" "$port" "ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa &&  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys" <<< y > /dev/null

}

function share_ssh_key() {
    local src_user=$1
    local src_ip=$2
    local src_passwd=$3
    local src_port=$4
    local src_nickname=$5
    local dst_user=$6
    local dst_ip=$7
    local dst_passwd=$8
    local dst_port=$9
    local dst_nickname=$10
    local dst_home=$(remote_cmd "$dst_user" "$dst_ip" "$dst_passwd" "$dst_port" "echo \$HOME")

    echo "*** share ssh-key ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_nickname}(${dst_user}@${dst_ip}:${dst_port})"
    echo "sshpass -p ${dst_passwd} scp -P ${dst_port} -o StrictHostKeyChecking=no ~/.ssh/id_rsa.pub ${dst_user}@${dst_ip}:/${dst_home}/.ssh/${src_nickname}.pub"

    remote_cmd "$src_user" "$src_ip" "$src_passwd" "$src_port" "sshpass -p ${dst_passwd} scp -P ${dst_port} -o StrictHostKeyChecking=no ~/.ssh/id_rsa.pub ${dst_user}@${dst_ip}:/${dst_home}/.ssh/${src_nickname}.pub"

    remote_cmd "$dst_user" "$dst_ip" "$dst_passwd" "$dst_port" "cat ~/.ssh/${src_nickname}.pub >> ~/.ssh/authorized_keys"
}

function set_authority() {
    local user=$1
    local ip=$2
    local passwd=$3
    local port=$4
    local nickname=$5

    echo "set authority ${nickname}(${user}@${ip}:${port})"

    remote_cmd "$user" "$ip" "$passwd" "$port" "chmod 400 ~/.ssh/authorized_keys"
}

function validate_ssh_conn() {
    local src_user=$1
    local src_ip=$2
    local src_passwd=$3
    local src_port=$4
    local src_nickname=$5
    local dst_user=$6
    local dst_ip=$7
    local dst_port=$8
    local dst_nickname=$9

    echo -e "\n*** validate ssh-connection ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_nickname}(${dst_user}@${dst_ip}:${dst_port})"

    remote_cmd "$src_user" "$src_ip" "$src_passwd" "$src_port" "ssh ${dst_user}@${dst_ip} -p ${dst_port} 'echo 2>&1' && echo '[SSH CONNECTION SUCCESS] ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_nickname}(${dst_user}@${dst_ip}:${dst_port})' || echo '[SSH CONNECTION FAILURE] ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_user}@${dst_nickname}(${dst_ip}:${dst_port})'"

}

function main() {
    # client
    if [ ! -z $CLIENT ]; then
        echo "CLIENT SETTING 1"
        ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa <<< y > /dev/null
        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
        chmod 400 ~/.ssh/authorized_keys
    fi

    # server: sshpass 설치 및 ssh-keygen
    for ((i=0; i<${#SERVER_LIST[@]}; i++))
    do
        local user=${!SERVER_LIST[i]:0:1}
        local ip=${!SERVER_LIST[i]:1:1}
        local passwd=${!SERVER_LIST[i]:2:1}
        local port=${!SERVER_LIST[i]:3:1}
        local nickname=${!SERVER_LIST[i]:4:1}
        # sshpass 설치
        install_sshpass "$user" "$ip" "$passwd" "$port"
        # ssh-keygen
        ssh_keygen "$user" "$ip" "$passwd" "$port" "$nickname"
    done

    # server: 각 서버 간 public key 공유
    for ((i=0; i<${#SERVER_LIST[@]}; i++))
    do
        local src_user=${!SERVER_LIST[i]:0:1}
        local src_ip=${!SERVER_LIST[i]:1:1}
        local src_passwd=${!SERVER_LIST[i]:2:1}
        local src_port=${!SERVER_LIST[i]:3:1}
        local src_nickname=${!SERVER_LIST[i]:4:1}

        # client
        if [ ! -z $CLIENT ]; then
            echo "CLIENT SETTING 2"
            local client_nickname=${CLIENT[4]}
            sshpass -p "$src_passwd" scp -P "$src_port" -o StrictHostKeyChecking=no ~/.ssh/id_rsa.pub "${src_user}"@"${src_ip}":/$(echo \$HOME)/.ssh/${client_nickname}.pub
            remote_cmd "$src_user" "$src_ip" "$src_passwd" "$src_port" "cat ~/.ssh/${client_nickname}.pub >> ~/.ssh/authorized_keys"
        fi

        for ((j=0; j<${#SERVER_LIST[@]}; j++))
        do
            local dst_user=${!SERVER_LIST[j]:0:1}
            local dst_ip=${!SERVER_LIST[j]:1:1}
            local dst_passwd=${!SERVER_LIST[j]:2:1}
            local dst_port=${!SERVER_LIST[j]:3:1}
            local dst_nickname=${!SERVER_LIST[j]:4:1}
            if [[ "${src_ip}:${src_port}" != "${dst_ip}:${dst_port}" ]]; then
                    share_ssh_key "$src_user" "$src_ip" "$src_passwd" "$src_port" "$src_nickname" "$dst_user" "$dst_ip" "$dst_passwd" "$dst_port" "$dst_nickname"
            fi
        done
    done

    # authorized_keys 보안
    for ((i=0; i<${#SERVER_LIST[@]}; i++))
    do
        local user=${!SERVER_LIST[i]:0:1}
        local ip=${!SERVER_LIST[i]:1:1}
        local passwd=${!SERVER_LIST[i]:2:1}
        local port=${!SERVER_LIST[i]:3:1}
        local nickname=${!SERVER_LIST[i]:4:1}

        set_authority "$user" "$ip" "$passwd" "$port" "$nickname"
    done

    # 잘 접속되는지 최종 확인
    for ((i=0; i<${#SERVER_LIST[@]}; i++))
    do
        local src_user=${!SERVER_LIST[i]:0:1}
        local src_ip=${!SERVER_LIST[i]:1:1}
        local src_passwd=${!SERVER_LIST[i]:2:1}
        local src_port=${!SERVER_LIST[i]:3:1}
        local src_nickname=${!SERVER_LIST[i]:4:1}

        for ((j=0; j<${#SERVER_LIST[@]}; j++))
        do
            local dst_user=${!SERVER_LIST[j]:0:1}
            local dst_ip=${!SERVER_LIST[j]:1:1}
            local dst_port=${!SERVER_LIST[j]:3:1}
            local dst_nickname=${!SERVER_LIST[j]:4:1}

            if [[ "${src_ip}:${src_port}" != "${dst_ip}:${dst_port}" ]]; then
                validate_ssh_conn "$src_user" "$src_ip" "$src_passwd" "$src_port" "$src_nickname" "$dst_user" "$dst_ip" "$dst_port" "$dst_nickname"
            fi
        done
    done
}


main

```

## Client: Java 설치

### .bash_profile에 Java 환경변수 등록

```sh
# .bash_profile
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_202.jdk/Contents/Home
export JRE_HOME=${JAVA_HOME}/jre
```

## Master, Worker: Java 설치

```sh
$ sudo yum install java-1.8.0-openjdk -y

# java 어디에 설치되었는지 확인
$ readlink -f $(which java)
```
- 환경변수 등록

```sh
$ vi ~/.bashrc
```

```sh
# ~/.bashrc
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.322.b06-1.el7_9.x86_64
```

```sh
$ source ~/.bashrc
```

## Master, Worker: Python 설치

```sh
$ yum install python3.6 -y
```

- CentOS 7의 yum이 python2를 사용하기 때문에 좋은 방법은 아니지만, 일단 혼자 쓰는 거니까 편의상 python 명령어를 python2.7가 아닌 python3.6에 연결 (참고로 python3.6은 Spark 3.2.0에서 deprecated 됨)

```sh
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
$ sudo update-alternatives --config python
```

- pip 명령어도 pip2.7이 아닌 pip3.6에 연결

```sh
$ sudo rm /usr/bin/pip
$ sudo ln -s /usr/bin/pip3.6 /usr/bin/pip
$ pip -V
```


## Client, Master, Worker: Spark 설치

### Spark 다운로드

```sh
$ wget https://dlcdn.apache.org/spark/spark-3.1.3/spark-3.1.3-bin-hadoop3.2.tgz
$ tar -xvzf spark-3.1.3-bin-hadoop3.2.tgz
$ mv spark-3.1.3-bin-hadoop3.2.tgz spark
```

### Spark 환경변수 등록

```sh
# MacOS
vi ~/.bash_profile

# CentOS 7
vi ~/.bashrc
```

```sh
# MacOS(Client): ~/.bash_profile
export BASE_DIR=/Users/a
export SPARK_HOME=${BASE_DIR}/spark
export PYSPARK_PYTHON=/opt/anaconda3/envs/pyspark/bin/python
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook'

# CentOS 7(Master, Worker): ~/.bashrc
export BASE_DIR=/home/a
export SPARK_HOME=${BASE_DIR}/spark
```

```sh
# MacOS
$ source ~/.bash_profile

# CentOS 7
$ source  ~/.bashrc
```

### Spark 세부 설정 및 튜닝
- 물리적인 Worker가 9개, Worker들의 cores 수가 8개니까 대략적으로 Worker 당 instance 2개, instance 당 4 cores씩 배분하면 적절할 것으로 보임
    - SPARK_EXECUTOR_INSTANCES=18
    - SPARK_EXECUTOR_CORES=4
- 물리적인 메모리는 Worker 당 32G인데, instance가 2개 뜨니까, 좀 여유남기고 13G 정도만 배정
    - SPARK_EXECUTOR_MEMORY=13G
- SPARK_DRIVER_CORES는 cluster mode에서만 사용되고 기본값이 1인데 SPARK_EXECUTOR_CORES와 같게 하는 것이 일반적(권장), client 모드로만 쓸 예정이지만 혹시 몰라 지정해 둠
    - SPARK_DRIVER_CORES=4

```sh
echo 'spark.master  spark://lab100:7077' >>  spark/conf/spark-defaults.conf
echo 'spark.serializer  org.apache.spark.serializer.KryoSerializer' >>  spark/conf/spark-defaults.conf

echo 'export SPARK_MASTER_IP="192.168.0.100"' >> spark/conf/spark-env.sh
echo 'export SPARK_MASTER_HOST=lab100'  >> spark/conf/spark-env.sh
echo 'export SPARK_MASTER_WEBUI_PORT=7777'  >> spark/conf/spark-env.sh
echo 'export SPARK_DRIVER_CORES=4' >> spark/conf/spark-env.sh
echo 'export SPARK_DRIVER_MEMORY=5G' >> spark/conf/spark-env.sh
echo 'export SPARK_EXECUTOR_INSTANCES=6'  >> spark/conf/spark-env.sh
echo 'export SPARK_EXECUTOR_CORES=4'      >> spark/conf/spark-env.sh
echo 'export SPARK_EXECUTOR_MEMORY=13G'  >> spark/conf/spark-env.sh
echo 'export SPARK_WORKER_DIR=$SPARK_HOME/work' >> spark/conf/spark-env.sh
echo 'export SPARK_LOG_DIR=$SPARK_HOME/logs'   >> spark/conf/spark-env.sh
echo 'export SPARK_PID_DIR=/tmp/spark/pid'   >> spark/conf/spark-env.sh
echo 'export SPARK_LOCAL_DIRS=/tmp/spark/scratch'   >> spark/conf/spark-env.sh

# echo 'export PYSPARK_PYTHON=/home/a/anaconda3/envs/pyspark/bin/python'   >> spark/conf/spark-env.sh
# echo 'export PYSPARK_DRIVER_PYTHON=/home/a/anaconda3/envs/pyspark/bin/python'   >> spark/conf/spark-env.sh

echo 'lab101' >> spark/conf/slaves
echo 'lab102' >> spark/conf/slaves
echo 'lab103' >> spark/conf/slaves
echo 'lab104' >> spark/conf/slaves
echo 'lab105' >> spark/conf/slaves
echo 'lab106' >> spark/conf/slaves
echo 'lab107' >> spark/conf/slaves
echo 'lab108' >> spark/conf/slaves
echo 'lab109' >> spark/conf/slaves
```

## Master: Spark Cluster 실행

```sh
# 시작
$ spark/sbin/start-all.sh

# 중지
$ spark/sbin/stop-all.sh
```

- <SPARK_MASTER_IP>:<SPARK_MASTER_WEBUI_PORT>로 접속하여 spark cluster가 잘 구성되었는지 확인
    - http://192.168.0.100:7777


## Client: Jupyter Notebook 세팅
- Client에서는 편의상 Conda로 Python을 관리

### Jupyter Notebook 설치

```sh
$ pip install jupyterlab
$ pip install jupyternotebook
```

### Jupyter Notebook 원격 접속 설정

```sh
$ jupyter-lab --generate-config
$ jupyter notebook --generate-config

$ vi ~/.jupyter/jupyter_notebook_config.py
```

```sh
# ~/.jupyter/jupyter_notebook_config.py
c = get_config()

c.NotebookApp.allow_origin = '*'
c.NotebookApp.ip = '*'

# Port 변경
c.NotebookApp.port = 9988

# 창으로 바로 안뜨게
c.NotebookApp.open_browser = False

# jupyter 시작 위치 설정
c.NotebookApp.notebook_dir = '/Users/a'

# 패스워드 설정: 이건 여기선 안함, 실행 명령어에 옵션으로 지정할 예정
# c.NotebookApp.password_required = True
# c.NotebookApp.password = ''
```

### Conda pyspark kernel 생성

```sh
$ conda create -n pyspark python=3.6 anaconda

$ conda activate pyspark

(pyspark)$ which python
/opt/anaconda3/envs/pyspark/bin/python
```

### sparksql-magic 설치

```sh
(pyspark)$ pip install sparksql-magic
```

#### sparksql-magic 사용법
- jupyter notebook 셀에서

```sh
%load_ext sparksql_magic
```

```sh
%%sparksql
SELECT * FROM dfTable
```

### Conda pyspark kernel 설정

```sh
# /home/a/anaconda3/envs/pyspark/share/jupyter/kernels/python3/kernel.json
$ vi ~/Library/Jupyter/kernels/pyspark/kernel.json
```

```sh
{
 "argv": [
  "/opt/anaconda3/envs/pyspark/bin/python",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "pyspark",
 "language": "python",
 "env": {
    "SPARK_HOME": "/Users/a/spark",
    "PYTHONPATH": "/Users/a/spark/python/:/Users/a/spark/python/lib/py4j-0.10.9.3-src.zip:/Users/a/spark/python/lib/pyspark.zip",
    "PYSPARK_PYTHON": "/usr/bin/python",
    "PYSPARK_DRIVER_PYTHON": "jupyter",
    "PYSPARK_DRIVER_PYTHON_OPTS": "notebook",
    "PYTHONSTARTUP": "/Users/a/spark/python/pyspark/shell.py",
    "PYSPARK_SUBMIT_ARGS": "--master=spark://<SPARK_MASTER_IP>:<SPARK_MASTER_PORT> --name 'pyspark.jupyter' --packages org.apache.hadoop:hadoop-aws:3.1.3,com.amazonaws:aws-java-sdk-bundle:1.11.563,com.google.guava:guava:27.1-jre  --deploy-mode client pyspark-shell"
 }
```

### 시작 시 자동 import 설정

```sh
$ ipython profile create
```

```
[ProfileCreate] Generating default config file: '/home/a/.ipython/profile_default/ipython_config.py'
[ProfileCreate] Generating default config file: '/home/a/.ipython/profile_default/ipython_kernel_config.py'
```

```sh
$ vi /home/a/.ipython/profile_default/startup/00-first_.py
```

```sh
import pandas as pd
import numpy as np
```


### Jupyter Notebook 실행 Script: start-lab.sh

```sh
#!/bin/bash

JUPYTER_PASSWORD=<JUPYTER_PASSWORD>

source /opt/anaconda3/etc/profile.d/conda.sh
conda activate pyspark
jupyter-lab --ip 0.0.0.0 --port <JUPYTER_PORT> --NotebookApp.password=$(echo $JUPYTER_PASSWORD | python3 -c 'from IPython.lib.security import passwd;print(passwd(input()))') --allow-root --no-browser &
```

```sh
$ bash start-lab.sh
```