---
layout: post
title: '[ShellScript] 기초 문법'
category: Linux
tags: [linux, shell script]
comments: true
---


# 공통 셀 출력 명령: echo
> 형식: echo [옵션] [문자열]

> 옵션: -n : 문자열의 마지막에서 줄 바꿈을 하지 않음

~~~bash
echo centos
echo "I Love Linux"
~~~

# 출력형식 지정 명령: printf
> 기능: 출력형식을 지정하여 문자열을 출력
> 형식: printf [옵션] [문자열]
> 옵션: %d, \n 등 c 언어의 printf() 함수를 지정

~~~bash
printf CentOS
printf "Hava a \n good time"
printf "%d + %d = %d \n" 10 20 30
~~~

# 숫자 계산

~~~bash
#!/bin/sh
echo "================================================"
echo "[예제 11-5] 입력받은 두 정수의 곱셈/나눗셈/평균값 산출 프로그램 "
echo "------------------------------------------------"
echo "1. 첫 번째 정수 입력 "
read num1
echo "2. 두 번째 정수 입력 "
read num2
echo "------------------------------------------------"
echo "> 입력한 두 정수의 값 "
echo " num1 = $num1 "
echo " num2 = $num2 "
echo "================================================"
echo "> 곱셈연산 결과 "
gob=`expr $num1 \* $num2`
echo " $num1 * $num2 = $gob "
echo "> 나눗셈연산 결과 "
na=`expr $num1 / $ num2`
echo " $num1 / $num2 = $na"
echo "> 두 수의 평균값 "
avg=`expr \($num1 + $num2\) / 2`
echo "$num1 + $num2 = $avg"
echo "================================================"

exit 0
~~~

# 파라미터 변수

> mapping.sh

~~~bash
#!/bin/sh
echo "================================================"
echo "[예제 11-6] 수행할 명령에 대한 파라미터 맵핑 프로그램"
echo "------------------------------------------------"
echo "1. 실행 파일이름 : <$0>: "
echo "2. 첫번째 파라미터는 <$1> 이고"
echo " 두번째 파라미터는 <$2> 입니다."
echo "3. 전체 파라미터는 <$*> 입니다."
echo "------------------------------------------------"
echo "> 프로그램을 종료합니다."
echo "================================================"

exit 0
~~~


# 분기문과 관계 연산자

## 기본 if문

### 기본 if문 문법구조

~~~bash
if [조건식]
then
    조건식이 참일 경우 실행할 문장
fi
~~~

## 조건연산자
> -eq  -neq  -gt  -ge  -lt  -le  !

## 논리연산자
> if [A] && [B] 동일 if [A] -a [B]
> if [A] || [B] 동일 if [A] -o [B]

### 기본 if문 사용예시

~~~bash
#!/bin/sh
if [ "space" = "space" ]
then
    echo "두 문자열은 같은 문자열입니다."
fi
~~~

~~~bash
#!/bin/sh

echo "==============================================="
echo "[예제 11-7] 기본 if문 문법형태 사용 프로그램"
echo "-----------------------------------------------"

if [ "space" = "space" ]
then
    echo " > 문자열 비교 : space = space "
    echo " > 두 문자열을 비교 연산자로 판단한 결과 "
    echo " > 주어진 조건은 <참> 입니다. "
fi


echo "------------------------------------------------"
echo ">> 프로그램을 종료합니다."
echo "================================================"

exit 0
~~~

# if~else문

~~~bash
#!/bin/sh
f_name=/lib/systemd/system/httpd.service
if [ -f $f_name ]
then
    echo "exists!"
else
    echo "not exists!"
fi
~~~

# case ~ esac문

## 문법구조

~~~bash
case 파라미터 또는 키보드 입력값 in
    조건1)
        조건1에 해당할 경우 실행할 명령
    조건2)
        조건2에 해당할 경우 실행할 명령
    조건n)
        조건n에 해당할 경우 실행할 명령
    *)
        앞에서 주어진 조건이외의 모든 경우 실행할 명령
esac
~~~



## 예제
> case_01.sh

~~~bash
#!/bin/sh
echo "====================================================="
echo "[예제 11-11] case-esac문을 사용하여 계절판별 프로그램"
echo "-----------------------------------------------------"
echo " > 명령 수행 파라미터 (Spring/Summer/Fall/Winter)"
echo ""
case "$1" in
    Spring)
        echo " >> 봄을 선택하셨습니다.";;
    Summer)
        echo " >> 여름을 선택하셨습니다.";;
    Fall)
        echo " >> 가을을 선택하셨습니다.";;
    Winter)
        echo " >> 겨울을 선택하셨습니다.";;
    *)
        echo " >> 계절을 의미하는 단어가 아닙니다."
        echo " >> 프로그램을 다시 시작하세요.";;
esac
echo "-----------------------------------------------------"
echo " >>> 프로그램을 종료합니다."
echo "====================================================="

exit 0
~~~

> case_02.sh

~~~bash
#!/bin/sh
echo "====================================================="
echo "[예제 11-12] case-esac문을 사용하여 심리분석 프로그램"
echo "-----------------------------------------------------"
echo "> 심리 상태 입력 : (Yes/No/Y/N)"
read mind
case $mind in
Yes | YES | yes | Y | y)
    echo "> Yes을 선택하셨습니다.";;
[nN]*)
    echo "> No 를 선택하셨습니다.";;
*)
    echo "> 알파벳은 대/소문자 구별 없이 Y또는 N을 입력하세요.";;
esac
echo "-----------------------------------------------------"
echo " >>> case-esac문 심리분석 프로그램을 종료합니다."
echo "====================================================="

exit 0
~~~

~~~bash
#!/bin/sh
birth_date=$1

case $birth_date in
    ??03??|??04??|??05??)
        echo "봄에 태어나셨네요";;
    ??06??|??07??|??08??)
        echo "여름에 태어나셨네요";;
    ??09??|??10??|??11??)
        echo "가을에 태어나셨네요";;
    ??01??|??02??|??12??)
        echo "겨울에 태어나셨네요";;
esac
~~~

~~~bash
#!/bin/sh
read val
echo $val
case $val in
    [0-9]*)
        echo "number";;
    [^0-9]*)
        echo "not number";;
esac
~~~

~~~bash
#!/bin/sh

case "$(date +%a)" in
    Sat|Sun)
        echo "주말엔 일 안합니다";
        exit;
esac
~~~

# 관계 연산자
> ifandor.sh

~~~bash
#!/bin/sh
echo "==============================================================="
echo "[예제 11-13] if~else문과 관계 연산자를 사용하여 파일 찾기 프로그램"
echo "---------------------------------------------------------------"
echo "> 찾고자 하는 파일 이름을 입력하세요."
read file_name
echo "---------------------------------------------------------------"

if [ -f $file_name ] && [ -s $file_name ] ; then
    echo "> 찾은 파일이름: $file_name"
    echo "> 찾은 파일의 내용 중 처음부터 8행의 내용은 다음과 같습니다."

echo "---------------------------------------------------------------"
    head -8 $file_name
else
    echo "> 찾았던 파일이름: $file_name"
    echo "> 이 파일은 존재하지 않거나 파일 크기가 0인 파일입니다."

echo "---------------------------------------------------------------"
echo " >>> 프로그램을 종료합니다."
echo "==============================================================="

exit 0
~~~

# 반복문

## 반복문 문법구조

~~~bash
for 변수 in 값1, 값2, 값2
do
    반복실행할 문장들
done

for  변수 in `seq 1 100`
do
    반복실행할 문장들
done

for (( i=1; i<=5; i++ ))
do
    반복실행할 문장들
done
~~~

> for_01.sh

~~~bash
#!/bin/sh
echo "========================================================"
echo "[예제 11-14] for문으로 1~10까지 누적합 산출 프로그램"
echo "--------------------------------------------------------"
sum=0

for (( cnt=1; cnt<=10; cnt++ ))
do
    sum=`expr $sum + $cnt`
echo "> 1부터 $cnt까지 누적합 ... $sum"
done

echo "---------------------------------------------------------"
echo "> 1 + 2 + .... + 9 + 10 ..............[$sum]"
echo ">>> 프로그램을 종료합니다"
echo "========================================================="
exit 0
~~~


> for_02.sh

~~~bash
#!/bin/sh
echo "=========================================================="
echo "[예제 11-15] 현재 디렉터리에 있는 파일 내용 출력 프로그램"
echo "----------------------------------------------------------"

for file_name in $(ls *.sh)
do
    echo "-----------------<$file_name>-------------------------"
    echo "> 파일 [$file_name]의 내용 중에서 처음부터 2행 출력"
    head -2 $file_name
done

echo "-----------------------------------------------------------"
echo ">>> 프로그램을 종료합니다"
echo "==========================================================="
exit 0
~~~

# while 문

## 조건연산자
> -eq -neq -gt -ge -lt -le !

> while_01.sh

~~~bash
#!/bin/sh
echo "==========================================================="
echo "[예제 11-16] while문으로 1~10까지 누적합 산출 프로그램"
echo "-----------------------------------------------------------"

sum = 0
cnt = 1

while [ $cnt -le 10 ]
do
    sum=`expr $sum + $cnt`
    echo "> 1부터 $cnt까지 누적합 ... $sum"
    cnt=`expr $cnt + 1`
done

echo "-----------------------------------------------------------"
echo "> 1 + 2 + 3 + ... + 9 + 10 .................[$sum]"
echo ">>> 프로그램을 종료합니다!"
echo "==========================================================="
exit 0
~~~

> while_02.sh

~~~bash
#!/bin/sh
echo "==========================================================="
echo "[예제 11-17] while문으로 비밀번호 일치여부 판정 프로그램"
echo "-----------------------------------------------------------"
echo "> 비밀번호를 입력하세요"
read psword
echo "-----------------------------------------------------------"

while [ $psword != "123456" ]
do
    echo "> 입력한 비밀번호 [$psword]는(은) 일치하지 않습니다..."
    echo "> 다시 입력하세요. "
    read psword
done

echo "------------------------------------------------------------"
echo "> 비밀번호 [$psword]이(가) 확인되었습니다."
echo ">>> 프로그램을 종료합니다."
echo "============================================================"
exit 0
~~~

# until문

## until문 문법구조

~~~bash
#!/bin/sh
until [조건식]
do
    조건식이 거짓을 경우 실행할 문장
done
~~~


## while문으로 작성한 while_01.sh 파일

~~~bash
while [ $cnt -le 10 ]
do
    sum=`expr $sum + $cnt`
    echo "> 1부터 $cnt까지 누적합 ... $sum "
    cnt = `expr $cnt + 1`
done
~~~


## until문으로 작성한 while_01.sh 파일

~~~bash
until [ $cnt -gt 10 ]
do
    sum=`expr $sum + $cnt`
    echo "> 1부터 $cnt까지 누적합... $sum"
    cnt=`expr $cnt + 1`
done
~~~

# break, continue, exit, return문

~~~bash
#!/bin/sh
echo "======================================================="
echo "[예제 11-18] break, continue, exit문 활용 프로그램"
echo "-------------------------------------------------------"
echo ">대/소문자 구별없이 제시된 알파벳을 선택하여 입력하세요."
echo ">선택할 알파벳 [b:break/c:contitnue/e:exit] "
echo "-------------------------------------------------------"
while [ 1 ]
do
    echo "> 알파벳을 입력 : "
    read choice
    case $choice in
        b : B)
            echo "> 반복문을 빠져나가면서 종료됩니다. " ; break;;
        c : C)
            echo "> continue문은 조건식으로 돌아갑니다. " ; continue;;
        e : E)
            echo "> exit문은 프로그램을 완전히 종료합니다. " ; exit 0;;
        *)
            echo "> 알파벳은 대/소문자 구별 없이 b, c, e만 허용합니다. "
        continue;;
    esac;
done

echo "========================================================"
exit 0
~~~

# 함수

## 사용자 정의 함수 정의

~~~bash
함수 이름 ()
{
    수행하고자 하는 명령
    return
}
~~~

## 사용자 정의 함수 호출

~~~bash
#!/bin/sh
...
함수 이름 -> 함수를 호출하려는 위치에서 함수 이름을 코딩하면 호출됨
...
exit 0
~~~

> userfun_01.sh

~~~bash
#!/bin/sh
echo "=============================================================="
echo "[예제 11-19] 사용자 정의 함수에 대한 정의 및 호출 프로그램"
echo "--------------------------------------------------------------"
echo "> 함수를 호출하려면 먼저 함수를 정의해야 합니다."

myfun ()
{
echo "------------------------< myfun 함수 >------------------------"
echo " o.사용자 정의 함수 myfun()에 대한 정의 "
echo " o.여기는 myfun 함수의 영역입니다. "
echo " o.myfun 함수가 호출 되었습니다. "
echo "--------------------------------------------------------------"
}

echo "> 사용자 정의 함수를 호출 : myfun 함수 이름 코딩 "
echo ""
myfun
echo ""
echo "> 프로그램을 종료합니다"
echo "=============================================================="
exit 0
~~~

## 파라미터와 함께 함수의 호출방식

~~~bash
함수 이름 ()
{
    수행하고자하는 명령
    return
}
~~~

## 사용자 정의 함수 호출과 파라미터 선언

~~~bash
#!/bin/sh
...
함수 이름 $1 $2 ... -> 함수를 호출할 때 파라미터도 같이 선언
...
exit 0
~~~


> userfun_02.sh

~~~bash
#!/bin/sh
echo "======================================================="
echo "[예제 11-20] 파라미터를 사용하여 함수를 호출하는 프로그램"
echo "======================================================="

add()
{
    echo "------------------<add 함수>------------------------"
    echo "o. 두 수에 대한 덧셈 연산식을 수행하는 함수입니다."
    total=`expr $1 + $2`
    echo "o.연산결과: $num1 + $num2 = $total"
    echo "o.여기까지가 add함수 영역입니다."
    echo "----------------------------------------------------"
}

echo "> 키보드로 두 수를 차례대로 입력하세요."
echo "> 첫 번째 수 입력"
    read num1
echo "> 두 번째 수 입력"
    read num2
    add $num1 $num2
echo "> 프로그램을 종료합니다."
echo "========================================================"
exit 0
~~~

# 문자열을 명령문으로 인식: eval
- eval은 프로그램에서 문자열로 처리한 부분을 명령문으로 인식하여 명령을 수행한다.

~~~bash
#!/bin/sh
echo "========================================================="
echo "[예제 11-21] 문자열을 명령문으로 인식하는 프로그램"
echo "---------------------------------------------------------"
filename="ls /tmp"
echo ">echo : ls /tmp를 문자열로 인식"
echo "---------------------------------------------------------"
eval $filename
echo "> 프로그램을 종료합니다."
echo "========================================================="
exit 0
~~~

# 출력형식 지정: printf
- 출력형식을 지정하여 문자열이나 실수의 소수점 자리 수 지정, 강제 줄 바꿈 명령 등을 수행할 수 있다

~~~bash
#!/bin/sh
echo "========================================================="
echo "[예제 11-22] printf를 사용하여 출력형식 지정 프로그램"
echo "---------------------------------------------------------"
echo "> 키보드로 실수를 입력하세요."
read float_num
echo "> 키보드로 문자열을 입력하세요."
read input_string
echo "---------------------------------------------------------"
echo "> 실수를 5.2f 출력 형식을 지정하여 출력"
printf "%5.2f \n "$float_num
echo "---------------------------------------------------------"
echo "> 문자열 출력 형식을 지정하여 출력"
echo "1.변수에 겹따모표를 안 붙였을 때"
printf "%s \n "$input_string
echo "========================================================="
exit 0
~~~

~~~bash
#!/bin/sh
echo "========================================================="
echo "[예제 11-23] set과 $로 명령어 형식 지정 프로그램"
echo "> 오늘의 날짜 출력: $(date)"
set $(date)
echo "> 오늘의 년도 cnffur: $1"
echo "> 오늘의 월/일 출력 $2 $3"
echo "> 오늘의 요일 출력: $4"
echo "> 현재 시간 출력: $5"
echo "---------------------------------------------------------"
echo ">> 프로그램을 종료합니다."
echo "========================================================="
exit 0
~~~






