---
layout: post
title: '[Trouble linux] sudo 명령어로 root 권한 얻지 못할 때'
category: Trouble
tags: [trouble, linux, ubuntu, centos]
comments: true
---

# Environment
- CentOS 7

# Trouble 
- Linux에서 sudo 명령어로 root 권한 얻지 못할 때

# Shooting
- userid is not in the sudoers file. This incident will be reported. 에러가 뜰 때 root계정 sudoers 파일에서 user에 대한 권한을 부여하여 해결할 수 있다.

~~~shell
# /etc/sudoers를 연다.
$ visudo -f /etc/sudoers
~~~

~~~shell
# /etc/sudoers 파일에 userid ALL=(ALL)  ALL 부분을 추가한다.

# User privilege specification
root   ALL=(ALL)  ALL
userid ALL=(ALL)  ALL
~~~

## Adding SSH Key to autorized_keys: permission denided(publickey)

~~~shell
$ chmod 700 .ssh
$ sudo chmod 640 .ssh/authorized_keys

# 만약 안되면 추가로 진행
$ sudo chown $USER .ssh
$ sudo chown $USER .ssh/authorized_keys
~~~
