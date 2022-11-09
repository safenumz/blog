---
layout: post
title: '[Linux] 화면 출력 및 파일 내용 보기 (echo, cat, more)'
category: Linux
tags: [linux, echo, cat]
commets: true
---


# echo
- 화면에 텍스트를 출력하는 명령어이다. 간단히 파일을 만들 때 사용할 수 있고, 쉘 프로그램 수행 중간에 진행 상황을 화면상에 알려주기 위해서도 사용한다.

~~~shell
$ echo "Hello echo"
~~~

<pre>
Hello echo
</pre>

- 여러 라인의 메시지를 출력하기 위해서는 -e 옵션과 함께 문자열 중간에 특수문자 중 줄바꿈 문자인 \n를 사용한다.

~~~shell
$ echo -e "Hello\necho"
~~~

<pre>
Hello
echo
</pre>


## echo로 파일에 내용 추가하기
- a.html 파일 마지막에 'key'라는 문자열을 추가하고 싶으면 다음 명령어를 실행하면된다. >>가 아니라 > 로 바꾸면 a.html 파일의 내용이 전부 지워지고 'key'로 대체되게 된다. 

~~~shell
$ echo 'kei' >> ./a.html
~~~

~~~shell
# b.html 파일을 생성한다.
$ touch b.html

# b.html 파일에 내용을 기록한다.
$ echo -e "--- \nlayout:post \ntitle:실전 \ncategory:cate  \ntags:[tag] \ncomments:false \n---" > b.html
~~~

# cat
- 텍스트 파일의 내용을 보기 위해 cat 명령어를 사용한다.

## cat 실행 결과를 변수에 담기
- ' 기호가 이니라 ` 기호라는 것에 주의

~~~shell
$ v=`cat a.html`

# 한줄로 출력된다.
$ cat $v

# " "으로 묶어주면 \n이 되어서 출력된다. 
$ cat "$v"
~~~


## cat으로 파일 병합하기
- cat 명령과 리다이렉션 기호로 여러 파일을 연결(병합)하여 하나의 파일을 만듭니다.

~~~shell
# b.html과 a.html을 연결하여 c.html을 만든다
$ cat b.html a.html > c.html
~~~


# more
- cat 명령어의 경우 파일이 길어서 한 화면을 벗어나는 경우 자동으로 스크롤이 된다. 그래서 앞 부분의 내용을 볼 수 없다. more 명령어를 사용하면 화면 스크롤이 계속 진행되지 않고 한 화면(페이지)의 분량만큰 보여준다. 스페이스바를 누르면 다음 페이지를 볼 수 있다.

~~~shell
$ more c.html
~~~