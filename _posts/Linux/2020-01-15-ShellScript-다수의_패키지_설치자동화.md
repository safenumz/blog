---
layout: post
title: '[ShellScript] 다수 패키지 설치 자동화'
category: Linux
tags: [linux, shell script]
comments: true
---

# Ubuntu/CentOS 다수 패키지 설치 자동화 스크립트

## install_script.sh

~~~bash
#!/bin/bash

PROGRAM_LIST=$1
LOG_FILE_NAME=$2

CURRENT_PATH=`pwd`
LOG_FILE=$CURRENT_PATH/$LOG_FILE_NAME


function os_check() {

    if [[ $(cat /etc/*release) =~ "CentOS" ]]
    then
        echo "centos"
    elif [[ $(cat /etc/*release) =~ "Ubuntu" ]]
    then
        echo "ubuntu"
    else
        echo "[WARNING] it's not a centos or ubuntu"
        exit 0
    fi
}

function err_check() {
    status=$1
    program_name=$2
    
    if [ "$status" == "0" ]
    then
        echo "[SUCCESS] $program_name" >> $LOG_FILE
        echo "[SUCCESS] $program_name"
        return 0
    else
        echo "[FAILED] $program_name" >> $LOG_FILE
        echo "[FAILED] $program_name"
        return 2
    fi
}

OS=$(os_check)

if [ "$OS" == "centos" ]
then
    HEAD="yum"
elif [ "$OS" == "ubuntu" ]
then
    HEAD="apt-get"
fi

cat $PROGRAM_LIST | while read line
do
    $HEAD install -y "${line}"
    err_check $? "${line}"
done
~~~

## 실행

~~~
$ ./install_script.sh    program_list.txt   log_install.txt
~~~