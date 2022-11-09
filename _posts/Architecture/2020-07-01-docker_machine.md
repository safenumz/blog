---
layout: post
title: '[Docker] Mac에서 docker-machine 활용하기'
category: Architecture
tags: [docker, docker-machine]
comments: true
---

## Mac에서 docker-machine 설치

```sh
# cast 패키지 설치
$ brew install cask
# docker, docker-machine 설치
$ brew install docker docker-machine
$ brew install virtualbox --cask
```

## 접속중인 사용자에게 권한 부여
- sudo 명령어 없이 docker 명령어 사용하기 위해

```sh
$ sudo usermod -aG docker $USER 
$ sudo su - $USER
```

## docker-machine 생성

```sh
# docker-machine create --driver virtualbox --virtualbox-memory "4096" --virtualbox-hostonly-cidr "25.0.1.100/24" lab
$ docker-machine create --driver virtualbox default

# 목록 표시
$ docker-machine ls

# 실행 환경 목록 표시
$ docker-machine ls --filter driver=virtualbox --filter state=Running

# 실행 환경의 상태 확인
$ docker-machine status default

# 실행 환경의 URL 확인
```

## docker 명령을 사용할 시스템 설정

```sh
$ docker-machine env default

$ eval $(docker-machine env default)
```

## docker-machine ip 정보 확인

```sh
$ docker-machine ip default
```

## docker-machine 접속하기
- DOCKER_MACHINE_IP: **docker-machine ip default** 명령어로부터 확인
- 유저명: docker
- 초기 비밀번호: tcuser

```sh
$ ssh docker@$(docker-machine ip default)

$ docker-machine ssh default
```

## docker-machine에 파일 전송

```sh
$ docker-machine scp docker-compose.yml default:/home/docker
```

## Packages 설치 방법
- docker-machine은 TinyCore distribution 기반으로 작동
- TinyCore에서 tce-load은 다른 주요 distribution의 yum, apt-get 등의 역할을 하는 명령어

```sh
$ tce-load -wi vim.tcz
$ tce-load -wi nano.tcz
$ tce-load -i compiletc
$ tce-load -wi gcc
$ tce-load -wi make

# sshpass 설치
$ wget http://www.tinycorelinux.net/10.x/x86/tcz/src/sshpass/sshpass-1.05-source.tar.gz
$ tar -xvzf sshpass-1.05-source.tar.gz
$ cd sshpass-1.05/
$ ./configure
$ make
$ make install
```

## docker-machine start/stop

```sh
$ docker-machine stop

$ docker-machine start
```

## Trouble shooting
### 버전 문제로 docker-machine이 작동하지 않을 때

1. find all the machines with docker-machine ls
2. remove the ones you don't need with docker-machine rm -y <machineName>
3. find all the "host-only ethernet adapters" with VBoxManage list hostonlyifs
4. Remove the orphaned ones with VBoxManage hostonlyif remove <networkName>
5. Create a vbox folder in the etc directory with sudo mkdir
6. Create a file networks.conf in the vbox folder, for example by sudo touch

```sh
$ sudo vi /etc/vbox/networks.conf
```

place the below line there

```sh
* 0.0.0.0/0 ::/0
```

7. create a new machine with docker-machine create -d virtualbox <machineName>
8. Run the command eval $(docker-machine env <machineName>) to configure your shell

### 기타

```sh
$ brew upgrade --cask docker
$ rm -rf ~/.docker
$ killall Docker && open /Applications/Docker.app
$ sudo ifconfig vboxnet0 down && sudo ifconfig vboxnet0 up
$ sudo "/Library/Application Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh" restart
```

```sh
docker-machine regenerate-certs default
export DOCKER_CERT_PATH=/Users/a/.docker/machine/certs
cp -f ~/.docker/machine/certs/* ~/.docker/machine/machines/dev/
# bridge0 en0 lo0
$ docker network create -d ipvlan --subnet=192.168.0.0/24 --gateway=192.168.0.1 \ -o parent=[ex : eth0 / docker host의 ethernet device name] (mode 옵션 생략시 l2모드로 생성됨) ipvlan_test
$ docker network create -d ipvlan --subnet=192.168.0.0/24 --gateway=192.168.0.1 -o parent=enp2s0f0 ipvlan_test

docker run -dit --privileged --name centos7 --net=ipvlan_test --ip 192.168.0.98 centos:7 /sbin/init


docker network create -d ipvlan --subnet=192.168.50.0/24 --gateway=192.168.0.1 -o parent=enp0s8 ipvlan_test


docker network  create -d ipvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o ipvlan_mode=l2 -o parent=enp2s0f0 db_net

docker network  create  -d ipvlan --subnet=192.168.214.0/24 --subnet=10.1.214.0/24 -o ipvlan_mode=l3 ipnet210


docker network create -d macvlan -o macvlan_mode=bridge --subnet=192.168.0.0/24 --gateway=192.168.0.1 -o parent=en0 macvlan_bridge




docker network create -d macvlan -o macvlan_mode=bridge --subnet=192.168.0.0/24 --gateway=192.168.0.1 -o parent=enp2s0f0 macvlan_bridge
docker run -it --rm --net=macvlan_bridge --ip=192.168.0.200 alpine /bin/sh
ping 192.168.0.200
```