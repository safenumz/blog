---
layout: post
title: '[ShellScript] 파일 내용 동일성 비교 (diff)'
category: Linux
tags: [linux, shell script]
comments: true
---

# 파일 내용 동일성 비교

## diff_check.sh
비교해야 할 프로그램 리스트가 저장된 diff1.txt와 diff2.txt을 각각 한 줄씩 읽어들이면서 diff 명령어로 파일 내용의 동일성을 검증한다.

~~~bash
#!/bin/bash
  
FILE1=$1
FILE2=$2

# 파일 라인 수 확인
function max_line() {
    file_name1=$1
    file_name2=$2

    file_num1=`cat $file_name1 | wc -l`
    file_num2=`cat $file_name2 | wc -l`

    if (( $file_num1 >= $file_num2 ))
    then
        echo $file_num1
    else
        echo $file_num2
    fi
}

function read_file() {
    file_name=$1
    line_num=$2
    sed -n "${line_num}p" $file_name
}

max_num=$(max_line $FILE1 $FILE2)
echo "Max line num is ${max_num}"


for (( i=0; i<$max_num; i++ ))
do
    file1_line=$(read_file $FILE1 $i)
    file2_line=$(read_file $FILE2 $i)

    if diff $file1_line $file2_line > /dev/null
    then
        echo "Nothing changed: ${file1_line}, ${file2_line}"
    else
        echo "Something changed: ${file1_line}, ${file2_line}"
    fi
done
~~~

## diff1.txt

~~~bash
Untitled.ipynb
Untitled1.ipynb
Untitled11.ipynb
Untitled12.ipynb
Untitled2.ipynb
Untitled3.ipynb
Untitled4.ipynb
Untitled5.ipynb
Untitled6.ipynb
Untitled7.ipynb
Untitled8.ipynb
Untitled9.ipynb
~~~

## diff2.txt

~~~bash
Untitled.ipynb
Untitled1.ipynb
Untitled11.ipynb
Untitled12.ipynb
Untitled2.ipynb
Untitled3.ipynb
Untitled4.ipynb
Untitled.ipynb
Untitled6.ipynb
Untitled7.ipynb
Untitled8.ipynb
~~~

## 실행

~~~bash
$ diff_check.sh diff1.txt diff2.txt

Nothing changed: Untitled.ipynb, Untitled.ipynb
Nothing changed: Untitled1.ipynb, Untitled1.ipynb
Nothing changed: Untitled11.ipynb, Untitled11.ipynb
Nothing changed: Untitled12.ipynb, Untitled12.ipynb
Nothing changed: Untitled2.ipynb, Untitled2.ipynb
Nothing changed: Untitled3.ipynb, Untitled3.ipynb
Nothing changed: Untitled4.ipynb, Untitled4.ipynb
Something changed: Untitled5.ipynb, Untitled.ipynb
Nothing changed: Untitled6.ipynb, Untitled6.ipynb
Nothing changed: Untitled7.ipynb, Untitled7.ipynb
Nothing changed: Untitled8.ipynb, Untitled8.ipynb
~~~