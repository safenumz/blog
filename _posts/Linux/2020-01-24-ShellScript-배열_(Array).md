---
layout: post
title: '[ShellScript] 배열 (Array)'
category: Linux
tags: [linux, shell script, array]
comments: true
---


# 배열에 값 추가하기

~~~bash
#!/bin/bash

PLANETS=( "EARTH" "MARS" "VINUS" )

PLANETS[3]="SUN"

echo ${PLANETS[@]}
~~~

<pre>
EARTH MARS VINUS SUN
</pre>


~~~bash
#!/bin/bash

PLANETS=()
PLANETS+=("EARTH")
PLANETS+=("MARS")
PLANETS=(${PLANETS[@]} "VINUS")

echo ${PLANETS[@]}
~~~

<pre>
EARTH MARS VINUS
</pre>



# 배열의 값 출력하기

~~~bash
#!/bin/bash

PLANETS=( "EARTH" "MARS" "VINUS" )

# 인덱스 0부터 2개 출력하기
echo ${PLANETS[@]:0:2}

# 맨 마지막 값 출력하기
echo ${PLANETS[@]: -1}

# 맨 마지막 2개 출력하기
echo ${PLANETS[@]: -2}
~~~

<pre>
EARTH MARS
VINUS
MARS VINUS
</pre>




# 배열의 값 제거

~~~bash
#!/bin/bash

PLANETS=( "EARTH" "MARS" "VINUS" )

# 인덱스 1 값 제거
IDX=1
PLANETS_DEL=("${PLANETS[@]:0:$IDX}" "${PLANETS[@]:$(($IDX + 1))}")
echo ${PLANETS_DEL[@]}

unset PLANETS[1]

echo ${PLANETS[@]}

# 배열의 전체 값 제거 후 배열 개수 출력
unset PLANETS

echo ${#PLANES[@]}
~~~

<pre>
EARTH VINUS
EARTH VINUS
0
</pre>




# 배열의 값 패턴 추출

~~~bash
#!/bin/bash

PLANETS=( "EARTH" "MARS" "VINUS" "MAX" )

# MA로 시작하는 값 빼고 출력
echo ${PLANETS[@]/MA*/}

# 배열의 요소 변경하여 출력
echo ${PLANETS[@]/MARS/SATURN}
~~~

<pre>
EARTH VINUS
EARTH SATURN VINUS MAX
</pre>



# 배열 순환 (Array loop)

~~~bash
#!/bin/bash

for NAME in "ME" "YOU" "THEN" "ALL"
do
    echo "Name is ${NAME}"
done
~~~

<pre>
Name is ME
Name is YOU
Name is THEN
Name is ALL
</pre>


~~~bash
#!/bin/bash

PLANETS=( "EARTH" "MARS" "VINUS" )

for PLANET in ${PLANETS[@]}
do
    echo "This is ${PLANET}"
done
~~~

<pre>
This is EARTH
This is MARS
This is VINUS
</pre>

~~~bash
#!/bin/bash

PLANETS=( "EARTH" "MARS" "VINUS" )

for (( i=0; i<${#PLANETS[@]}; i++ ))
do
    echo "Planet #$i is ${PLANETS[i]}"
done
~~~

<pre>
Planet #0 is EARTH
Planet #1 is MARS
Planet #2 is VINUS
</pre>



# 배열과 함수 연동
- 배열 요소를 꺼낼 때는 !를 붙여줘서 배열 요소의 값을 참조하도록 함

## 배열을 함수에 전달하기

~~~bash
#!/bin/bash

function planets_func() {
    for planet in ${1}[@]
    do
        echo ${!planet}
    done
}


PLANETS=( "EARTH" "MARS" "VINUS" )

planets_func PLANETS
~~~

<pre>
EARTH MARS VINUS
</pre>

## 배열의 요소 함수에 전달하기

~~~bash
#!/bin/bash

function planets_func() {
    for planet in ${!1}
    do
        echo ${planet}
    done
}

PLANETS=( "EARTH" "MARS" "VINUS" )
planets_func PLANETS[@]
~~~

<pre>
EARTH
MARS
VINUS
</pre>



# 텍스트 파일의 내용을 배열로 불러오기

~~~bash
$ cat planets.txt
~~~

<pre>
ALIVE EARTH 
MARS
VINUS
</pre>

~~~bash
#!/bin/bash

# 기존 IFS 백업
IFS_backup="$IFS"

# IFS 값을 줄바꿈으로 변경
IFS=$'\n'

# 배열 할당
PLANETS=( `cat planets.txt` )

for PLANET in "${PLANETS[@]}"
do 
    echo "$PLANET"
done

# IFS 원상복구
IFS="$IFS_backup"
~~~

<pre>
ALIVE EARTH
MARS
VINUS
</pre>