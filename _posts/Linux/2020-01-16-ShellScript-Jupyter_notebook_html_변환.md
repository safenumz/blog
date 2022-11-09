---
layout: post
title: '[ShellScript] Jupyter Notebook을 jekyll 블로그에 업로드하기'
category: Linux
tags: [linux, shell script]
comments: true
---

# Jupyter Notebook 컨텐츠를 jekyll 블로그에 업로드하기 위한 자동화 스크립트
- jupyter notebook을 jekyll blog에 업로드하기 위한 자동화 스크립트이다. 반복적이고 번거로운 작업이라 자동화가 필요하다.

## blog_script.sh

~~~bash
#!/bin/bash

#===================================================================
# Script for uploading jupyter notebook to jekyll blog
# Author : Jason Ahn
# Path   : /
# Name   : blog_script.sh
# Usage  : /blog_script.sh   $1   $2   $3   $4   $5
#    $1  : /Users/a/notebook/배열.ipynb
#    $2  : "자료구조/배열"
#    $3  : "Algorithm"
#    $4  : "algorithm, array"
#    $5  : "/Users/a/onedrive/safenumero.github.io"
#===================================================================

#-------------------------------------------------------------------
# set sed to use the sed command in MacOS
#-------------------------------------------------------------------

if [ $(uname -s) == "Darwin" ]
then
    if [[ -z $(brew list | grep gnu-sed) ]]
    then
        brew install gnu-sed
    fi

    SED=gsed
else
    SED=sed
fi

#-------------------------------------------------------------------
# set parameter and path
#-------------------------------------------------------------------

if [ -f $1 ]
then
    #JUPYTER_PATH=${1%/*}
    JUPYTER_FILE=$1
    JUPYTER_NAME=${1##*/}
else
    echo "[ERROR] ipynb file does not exist!"
    exit 2
fi

TITLE_RAW=$2

CATEGORY=$3

TAGS=$4

# blog path setting
if [ -z $5 ]
then
    # default blog path
    BLOG_PATH=/Users/a/onedrive/git/safenumero.github.io/
else
    BLOG_PATH=$5
fi

if [ ! -d "$BLOG_PATH/_posts/$CATEGORY" ] 
then
    #mkdir -p "$BLOG_PATH/_posts/$CATEGORY"
    #echo "make directory: $BLOG_PATH/_posts/$CATEGORY"
    echo "[ERROR] category folder does not exist!"
    exit 2
fi

if [ ! -d "$BLOG_PATH/_ipynb/$CATEGORY" ]
then
    mkdir -p "$BLOG_PATH/_ipynb/$CATEGORY"
    echo "make directory: $BLOG_PATH/_ipynb/$CATEGORY"
fi


#-------------------------------------------------------------------
# convert ipynb to html
#-------------------------------------------------------------------

jupyter nbconvert --to html ${JUPYTER_FILE}


#-------------------------------------------------------------------
# set jekyll header
#-------------------------------------------------------------------

if [ ! -z $(echo "${JUPYTER_NAME:0:11}" | grep [1-2][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]-) ]
then
    if [ ! -z $(echo "${JUPYTER_NAME:11}" | grep -) ]
    then
        JUPYTER_TMP="$(echo "${JUPYTER_NAME:11}")"
        TITLE_HEADER="[${JUPYTER_TMP%%-*}] "
    	TITLE_DETAIL_TMP="$(echo "${JUPYTER_TMP#*-}" | $SED -e 's/^ *//' -e 's/ *$//')"
    	TITLE_DETAIL="$(echo ${TITLE_DETAIL_TMP%.*} | $SED 's/_/ /g')"
    else
        TITLE_HEADER=""
        TITLE_DETAIL_TMP="$(echo "${JUPYTER_NAME:11}" | $SED -e 's/^ *//' -e 's/ *$//')"
        TITLE_DETAIL="$(echo ${TITLE_DETAIL_TMP%.*} | $SED 's/_/ /g')"
    fi
else
    if [ ! -z $(echo "${JUPYTER_NAME}" | grep -) ]
    then
        TITLE_HEADER="[${JUPYTER_NAME%%-*}] "
        TITLE_DETAIL_TMP="$(echo "${JUPYTER_NAME#*-}" | $SED -e 's/^ *//' -e 's/ *$//')"
        TITLE_DETAIL="$(echo ${TITLE_DETAIL_TMP%.*} | $SED 's/_/ /g')"
    else
        TITLE_HEADER=""
        TITLE_DETAIL_TMP="$(echo "${JUPYTER_NAME}" | $SED -e 's/^ *//' -e 's/ *$//')"
        TITLE_DETAIL="$(echo ${TITLE_DETAIL_TMP%.*} | $SED 's/_/ /g')"
    fi
fi

if [[ ! -z $(echo $TITLE_RAW | grep /) ]]
then
    TITLE_HEADER_TMP="${TITLE_RAW%%/*}"
    TITLE_DETAIL_TMP="$(echo "${TITLE_RAW#*/}" | $SED -e 's/^ *//' -e 's/ *$//')"

    if [ ! "$TITLE_HEADER_TMP" == "" ]
    then
        TITLE_HEADER="[${TITLE_HEADER_TMP}] "
    else
        TITLE_HEADER=""
    fi
    
    if [ ! "$TITLE_DETAIL_TMP" == "" ]
    then
        TITLE_DETAIL="$TITLE_DETAIL_TMP"
    fi

else
    if [ ! "$TITLE_RAW" == "" ]
    then
        TITLE_HEADER=""
        TITLE_DETAIL="$TITLE_RAW"
    fi
fi

HEADER="---\nlayout: post\ntitle: '${TITLE_HEADER}${TITLE_DETAIL}'\ncategory: ${CATEGORY}\ntags: [${TAGS}]\ncomments: true\n---\n"

$SED   -i   '1  i\'"$HEADER"   ${JUPYTER_FILE%.*}.html


#-------------------------------------------------------------------
# move html and ipynb file
#-------------------------------------------------------------------

if [ -z $(echo "${JUPYTER_NAME:0:11}" | grep [1-2][0-9][0-9][0-9]-[0-1][0-9]-[0-3][0-9]-) ]
then
    TIMESTAMP=$(date "+%Y-%m-%d")
    mv   "${JUPYTER_FILE%.*}.html"   "${BLOG_PATH}/_posts/${CATEGORY}/${TIMESTAMP}-${JUPYTER_NAME%.*}.html"
    cp   -f   "${JUPYTER_FILE}"    "${BLOG_PATH}/_ipynb/${CATEGORY}/${TIMESTAMP}-${JUPYTER_NAME}"
else
    mv   "${JUPYTER_FILE%.*}.html"   "${BLOG_PATH}/_posts/${CATEGORY}/${JUPYTER_NAME%.*}.html"
    cp   -f   "${JUPYTER_FILE}"    "${BLOG_PATH}/_ipynb/${CATEGORY}/${JUPYTER_NAME}"
fi


#------------------------------------------------------------------
# upload to github jekyll blog
#------------------------------------------------------------------

cd $BLOG_PATH

git add .

git commit -m "upload contents to jekyll blog >>> ${TITLE_DETAIL}"

git push origin master

# upload to a jekyll blog
jekyll serve

~~~

## 실행

<pre>
---
layout: post
title: '[&lt;헤더&gt;] &lt;제목&gt;'
category: &lt;카테고리명&gt;
tags: [&lt;태그목록&gt;]
comments: true
---
</pre>

~~~bash
$ ./blog_script.sh  <jupyter_notebook_파일명>   <헤더/제목>   <카테고리명>   <태그목록>   <블로그경로>
~~~


