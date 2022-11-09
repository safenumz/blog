---
layout: post
title: '[ShellScript] 인자값 받기 (getopts)'
category: Linux
tags: [linux, shell script, getopts]
comments: true
---

# getopts

~~~bash
#!/bin/bash

## 도움말 출력하는 함수
help() {
    echo "splt [OPTIONS] FILE"
    echo "    -h         도움말 출력."
    echo "    -a ARG     인자를 받는 opt."
    echo "    -b ARG     인자를 받는 opt2."
    exit 0
}
while getopts "a:b:h" opt
do
    case $opt in
        a) arg_a=$OPTARG
          echo "Arg A: $arg_a"
          ;;
        b) arg_b=$OPTARG
          echo "Arg B: $arg_b"
          echo "$arg_b"
          ;;
        h) help ;;
        ?) help ;;
    esac
done
 
# getopt 부분 끝나고 난 후의 인자(FILE) 읽기
shift $(( $OPTIND - 1))
file=$1
echo "$file"
~~~