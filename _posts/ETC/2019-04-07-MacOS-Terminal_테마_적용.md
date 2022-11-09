---
layout: post
title: '[MacOS] Mac Terminal 테마 적용'
category: etc
tags: [mac, oh my zsh]
comments: true
---

# Environment
- MacOS 

# Mac Terminal 테마 적용
## zsh 설치

최신버전으로 설치한다.

~~~shell
$ brew install zsh
~~~

## oh my zsh 설치

~~~shell
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
~~~


## zsh theme를 agnoster로 변경

~/.zshrc를 열어보면 기본테마인 robbyrussell이 적용되어 있다. ZSH_THEME를 agnoster로 변경한다.

~~~shell
$ vi ~/.zshrc
~~~

~~~shell
ZSH_THEME="agnoster"
~~~

## 컬러 적용

터미널 > 환경설정 > 프로파일 > 텍스트 > ANSI 색상 부분을 수정하면 컬러를 변경할 수 있다.

## 폰트 적용

만약 폰트가 깨진다면 네이버에서 개발한 D2Coding을 다운받아 적용한다. 깨지는 현상이 사라진다. 
미널 > 환경설정 > 프로파일 > 텍스트 > 서체 부분을 D2Coding으로 적용하면 된다.


## 프롬프트에 표시되는 사용자이름 삭제 및 멀티라인 적용
테마를 적용하고나니 경로가 전부 표시되고 사용자 이름이 너무 긴 자리를 차지하여 명령어를 입력할 공간이 부족해지는 현상이 발생했다. 
.zshrc 파일을 수정하여 불필요한 사용자 이름을 제거하고 멀티라인을 적용하자.

~~~shell
$ vi ~/.zshrc
~~~

.zshrc 파일을 열고 아래 주소의 내용을 넣는다.

~~~shell
<주소 임시 삭제>
~~~

변경 사항을 적용하면 끝이다.

~~~shell
$ source ~/.zshrc
~~~
