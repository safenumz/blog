---
layout: post
title: '[git] .gitignore 적용하기'
category: DevOps
tags: [git]
comments: true
---

# Environment
- Windows 10
- PowerShell

# .gitignore 파일
- Project에서 원하지 않는 파일들을 Git에서 제외시키는 설정 파일
- 항상 최상위 Directory에 존재해야 함

## 문법

~~~sh
# : comments

# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
~~~

## 적용하기
- git cash를 삭제하고 .gitignore 파일을 포함하여 commit하면 된다

~~~sh
$ git rm -r --cached .
$ git add .
$ git commit -m "Apply .gitignore"
~~~
