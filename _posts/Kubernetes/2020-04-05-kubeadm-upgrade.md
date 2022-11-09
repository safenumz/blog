---
layout: post
title: '[Kubernetes] kubeadm upgrade'
category: Kuberntetes
tags: [kubernetes, kubeadm, upgrade]
comments: true
---

# [kubeadm upgrade](https://kubernetes.io/ko/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

- kubeadm, kubelet, kubectl을 1.22.4에서 1.23.3 버전으로 업그레이드
    - kubeadm: 클러스터를 부트스트랩하는 명령
    - kubelet: Pod와 Container 시작과 같은 작업을 수행하는 컴포넌트
    - kubectl: 클러스터와 통신하기 위한 커맨드 라인 유틸리티

## 문제
- 작업 클러스터 : k8s
- 마스터 노드의 모든 Kubernetes control plane및 node 구성 요소를 버전 1.23.3으로 업그레이드
- master 노드를 업그레이드하기 전에 drain 하고 업그레이드 후에 uncordon해야 함

## Control-plane Upgrade

```sh
# hk8s-m 서버 접속
$ ssh hk8s-m
$ sudo -i

# 업그레이드 할 버전 확인
$ sudo yum list --showduplicates kubeadm --disableexcludes=kubernetes | tail -5

# kubeadm 업그레이드
$ sudo yum install -y kubeadm-1.23.3-0 --disableexcludes=kubernetes 
$ kubeadm version
$ sudo kubeadm upgrade plan v1.23.3
$ sudo kubeadm upgrade apply v1.23.3

# 노드 드레인 : console이나 master에서 실행
$ kubectl drain hk8s-m --ignore-daemonsets

# kubelet과 kubectl 업그레이드
$ sudo yum install -y kubelet-1.23.3-0 kubectl-1.23.3-0 --disableexcludes=kubernetes
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet

# 노드 uncordon
$ kubectl uncordon hk8s-m

# 업그레이드 버전 확인
$ kubectl get nodes
```

##  Worker node Upgrade

```sh
# Upgrade할 node에 접속
ssh <node>

# kubeadm 업그레이드
$ sudo yum install -y kubeadm-1.23.3-0 --disableexcludes=kubernetes

# "kubeadm upgrade" 호출
$ sudo kubeadm upgrade node

# console로 빠져 나와서 노드 드레인
$ exit
$ kubectl drain <node> --ignore-daemonsets

# kubelet, kubeadm 업그레이드
$ ssh <node>
$ sudo yum install -y kubelet-1.23.3-0 kubectl-1.23.3-0 --disableexcludes=kubernetes
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet

# console로 빠져 나와서 노드 uncordon
$ exit
$ kubectl uncordon <node>
```

