---
layout: post
title: '[ShellScript] 에러메세지 로그 저장'
category: Linux
tags: [linux, shell script]
comments: true
---

# 에러메시지 로그 저장

## err_check_script.sh

~~~bash
#!/bin/bash

PYTHON_PATH=/anaconda3/bin/python3
MAIN_JOB=$1
shift
PARAMETER=$@


# 로그 파일 생성
function make_log_file() {
    HEAD=$1
    MID=$2
    while true
    do
        TIMESTAMP=`date "+%Y%m%d_%H%M%S"`
        LOG_FILE="${HEAD}_${MID}_${TIMESTAMP}.chk"
        if [ ! -f $LOG_FILE ]
        then
            touch $LOG_FILE
            echo $LOG_FILE
            break
        fi
    done
}

# 에러 체크 함수
function error_check() {
    status=$1
    
    if [ "$status" == "0" ]
    then
        return 0
    elif [ "$status" == "1" ]
    then
        err_time=`date "+%Y-%m-%d %H:%M:%S"`
        err_msg=`cat $err_tmp_file`
        LOG_FILE=$( make_log_file log $MAIN_JOB )
        
        echo "[ Log Time ]" >> $LOG_FILE
        echo "$err_time" >> $LOG_FILE
        echo "[ File Name ]"  >> $LOG_FILE
        echo "$MAIN_JOB"  >> $LOG_FILE
        echo "[ Error Message ]"  >> $LOG_FILE
        echo "$err_msg"  >> $LOG_FILE
        return 2
    fi
}


# Python MAIN_JOB 실행
err_tmp_file=$( make_log_file err $MAIN_JOB )
$PYTHON_PATH   $MAIN_JOB   $PARAMETER   2>   $err_tmp_file
error_check $?; status=$?
rm $err_tmp_file

exit $status
~~~

## 실행

~~~bash
$ err_check_script.sh test.py 1 chk
~~~