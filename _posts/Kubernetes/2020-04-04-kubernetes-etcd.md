---
layout: post
title: '[Kubernetes] ETCD Backup and Restore'
category: Kuberntetes
tags: [kubernetes, etcd, backup, restore]
comments: true
---

# [ETCD](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/)
- Coreos가 만든 분산 key:value 형태의 데이터 스토리지
- 쿠버네티스 클러스터의 정보를 저장(memory)해서 사용
- 모든 etcd 데이터는 etcd 데이터베이스 파일에 보관: **/var/lib/etcd**
- etcd 관리 명령: etcdctl
- etcdctl 설치 확인

```sh
# ETCD를 호스팅 할 시스템에 ssh 로그인
$ ssh k8s-master

# 동작중인 etcd 버전과 etcdctl 툴이 설치여부 확인
$ etcd --version
$ etcdctl version

# 쿠버네티스 동작에 필요한 namespace
$ kubectl get pod -n kube-system

$ cd /etc/kubenetes/manifests

$ cat etcd.yaml
```

## Backup
- master의 장애와 같은 예기치 못한 사고로 인해 ETCD 데이터베이스가 유실될 경우를 대비해서 Backup API를 제공
- **/var/lib/etcd** -> **/tmp/etcd_backup**

```sh
$ etcdctl snapshot save <SNAPSHOT_FILENAME>
```

```sh
# etcd를 호스팅 할 시스템에 ssh 로그인
$ ssh k8s-master

# etcd 설치 여부 확인 
 
$ etcdctl version

# trusted-ca-file 확인
$ ps -ef | grep kube | grep trusted-ca-file

# cert-file 확인
$ ps -ef | grep kube | grep cert-file

# key-file 확인
$ ps -ef | grep kube | grep key-file

$ sudo ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/server.crt \
    --key=/etc/kubernetes/pki/etcd/server.crt \
    snapshot save /tmp/etcd-backup

$ ls -l /tmp/
```

## Restore
- Sanpshot으로 저장한 database 파일을 동작중인 etcd에 적용하여 snapshot 생성시점으로 되돌리기
- 단계
    - snapshot 파일을 데이터베이스 파일로 복원
    - 동작중인 etcd Pod의 구성정보를 복원된 데이터베이스 위치로 수정 적용

```sh 
# /data/etcd-snapshot.db -> /var/lib/etcd-new/
$ etcdctl snapshot restore <SNAPSHOT_FILENAME>
```

```sh
$ sudo ETCDCTL_API=3 etcdctl --data-dir=/var/lib/ctcd-new snapshot restore /tmp/etcd-backup

$ sudo tree /var/lib/etcd-new/

# 수정해서 저장되면 자동으로 restart 됨
$ sudo vi /etc/kubernetes/manifests/etcd.yaml
# hostPath 부분 신규 경로로 수정

$ sudo docker ps -a | grep etcd

$ kubectl get pods

$ sudo ls /var/lib/etcd-new
$ sudo tree /var/lib/etcd-new
```