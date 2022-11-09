---
layout: post
title: '[Mesos] Mesos build'
category: Architecture
tags: [mesos, spark]
comments: true
---

# Environment
- CentOS 7

# Mesos build

## 바이너리 파일 다운 후 압축 풀기

~~~shell
$ wget
$ tar xvzf mesos
$ ln -s mesos mesos
~~~

## essential development tools과 other Mesos dependencies 설치

~~~shell
$ sudo yum groupinstall -y "Development Tools"

$ sudo yum install -y maven python-devel zlib-devel libcurl-devel openssl-devel cyrus-sasl-devel cyrus-sasl-md5 apr-devel subversion-devel apr-util-devel
~~~

## Buiding Mesos

~~~shell
$ cd mesos
$ mkdir build
$ ./bootstrap
$ cd build
$ ../configure
$ make
~~~

~~~shell
# Run test suite.
$ make check

# Install (Optional).
$ make install
~~~

~~~shell
# Change into build directory.
$ cd build

# Start mesos master (Ensure work directory exists and has proper permissions).
$ sudo mesos/build/bin/mesos-master.sh --ip=secondnode.hadoop.kr --work_dir=/var/lib/mesos

# Start mesos slave.
$ sudo mesos/build/bin/mesos-slave.sh --master=secondnode.hadoop.kr:5050 --work_dir=/var/lib/mesos

# Visit the mesos web page.
$ http://127.0.0.1:5050

# Run C++ framework (Exits after successfully running some tasks.).
$ ./src/test-framework --master=127.0.0.1:5050

# Run Java framework (Exits after successfully running some tasks.).
$ ./src/examples/java/test-framework 127.0.0.1:5050


# Run Python framework (Exits after successfully running some tasks.).
$ ./src/examples/python/test-framework 127.0.0.1:5050
~~~~
