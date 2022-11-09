---
layout: post
title: '[ShellScript] 파일 백업 및 복구'
category: Linux
tags: [linux, shell script]
comments: true
---

# 파일 백업 스크립트 

## backup_script.sh

~~~bash
#!/bin/bash

BACKUP_LIST=$1
BACKUP_FORDER_NAME=$2
OPTION=$3

CURRENT_PATH=`pwd`
BACKUP_PATH=${CURRENT_PATH}/${BACKUP_FORDER_NAME}

BACKUP_LOG=${CURRENT_PATH}/${BACKUP_FORDER_NAME}.txt
RECOVERY_LOG=${CURRENT_PATH}/rec_${BACKUP_FORDER_NAME}.txt

# backup
if [ "$OPTION" == "0" ]
then
    if [ ! -d $BACKUP_PATH ]
    then
        mkdir -p $BACKUP_PATH
    else
        echo "[ERROR] cannot create backup forder because such forder alreadly exists"
        exit 0
    fi    
    cat $BACKUP_LIST | while read line
    do
        if [ -f "${line}" ]
        then
            mkdir   -p   "${BACKUP_PATH}/${line%/*}"
            cp  -f   "$line"   "${BACKUP_PATH}/${line%/*}" 
            echo "[BACKUP SUCCESS] $line"
            echo "${BACKUP_PATH}/${line}" >> $BACKUP_LOG
        else
            echo "[ERROR] file to back up does not exist >>> $line"
            echo "[ERROR] ${BACKUP_PATH}/${line}" >> $BACKUP_LOG
        fi
    done

# recovery  
elif [ "$OPTION" == "1" ]
then
    cat $BACKUP_LIST | while read line
    do
        echo "${BACKUP_PATH}/${line}"
        if [ -f "${BACKUP_PATH}/${line}" ]
        then
            cp   -f   "${BACKUP_PATH}/${line}"   "${line%/*}"
            echo "[RECOVERY SUCCESS] ${BACKUP_PATH}/${line} -> $line"
            echo "[RECOVERY SUCCESS] ${BACKUP_PATH}/${line} -> $line" >> $RECOVERY_LOG
        else
            echo "[ERROR] failed to recover file >>> ${BACKUP_PATH}/${line}"
            echo "[ERROR] ${BACKUP_PATH}/${line} -> $line" >> $RECOVERY_LOG
        fi
    done
fi
~~~

## 실행

./backup_script.sh    <백업파일리스트>   <백업폴더명>   <백업: 0, 복구: 1> 

~~~bash
$ ./backup_script.sh    backup_list.txt   backup_forder   0 
~~~