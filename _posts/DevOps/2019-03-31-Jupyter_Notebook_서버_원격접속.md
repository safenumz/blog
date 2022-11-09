---
layout: post
title: '[Jupyter Notebook] 서버 원격접속'
category: DevOps
tags: [jupyter notebook, 원격접속]
comments: true
---

# Environment
- CentOS 7
- Ubuntu
- MacOS
- Python 3.6

# Jupyter Notebook 서버 원격접속

## 1. config 파일 만들기

~~~sh
$ jupyter notebook --generate-config
~~~

/home/username/.jupyter 디렉토리에 jupyter_notebook_config.py 파일이 생성됨

## 2. 서버 비밀번호 생성

~~~sh
$ python
~~~

~~~python
>>> from notebook.auth import passwd
>>> passwd()

Enter password:
Verify password: 

hd4 'sha1:8e91b38948a8:98542922acc9d7c595dba50ef3d8b070229e55ef'
hd5 'sha1:d950486de86a:72f20b66084a7c5a04867be6e17b1922f770e337' # 이런 식으로 입력한 비밀번호를 암호화 하여 반환해줍니다.
~~~

## 3. 주피터 서버 환경설정하기
/home/username/.jupyter 디렉토리에 가서 jupyter_notebook_config.py 파일을 vi 편집기로 연다.

~~~sh
$ vi /home/a/.jupyter/jupyter_notebook_config.py
~~~


~~~sh
c = get_config()
c.NotebookApp.allow_origin = '*' # 외부 접속 허용하기
c.NotebookApp.ip = 'hd5.cluster.kr' # 아이피 설정
c.NotebookApp.port = 58888 # 포트 설정
c.NotebookApp.password = u'sha1:d950486de86a:72f20b66084a7c5a04867be6e17b1922f770e337' # 비밀번호 설정
c.NotebookApp.open_browser = False # 시작시 브라우저 실행여부
c.NotebookApp.notebook_dir = '원하는/작업경로를/입력해/주세요' #작업경로 설정
~~~


## 4. Jupyter 서버 시작하기

~~~sh
source /home/a/.jupyter/jupyter_notebook_config.py
$ cd /home/a/.jupyter
$ jupyter notebook --config jupyter_notebook_config.py
~~~


## 5. 주피터 서버 외부에서 접속하기

브라우저에 <설정한 ip>:<설정한 port> 주소를 치면 원격접속이 가능하다


## 공유기 포트 열기 및 DHCP 서버 설정
외부포트가 열려있지 않으면 외부에서 접속할 수 없다. 공유기의 포트포워드 기능을 활용하여 주피터 노트북이 사용하는 내부포트 8888과 동일하게 외부포트도 8888로 열어준다. 이제 http://[외부 IP 주소]:8888 로 외부에서 jupyter notebook에 원격으로 접속 가능하다. 내부 IP 주소가 아니라 외부 IP 주소를 써야 한다. 맥주소를 확인하여 DHCP 서버주소 관리에 등록해준다. Ubuntu라면 ssh를 설치해 준다.

~~~shell
$ sudo apt-get install ssh
~~~
