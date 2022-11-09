---
layout: post
title: '[Kubernetes] 클러스터 목록 확인 및 클러스터 이동'
category: Kuberntetes
tags: [kubernetes]
comments: true
---

# 클러스터 목록 확인

```sh
$kubectl config get-context

# 현재 클러스터 확인
$ kubectl config current-context

# 노드 명 확인
$ kubectl get nodes | cut -d ' ' -f1 | grep -v NAME > /var/CKA2022/hk8s-node-info.txt

# ready 상태의 노드 이름만 추출
$ kubectl get nodes | grep -i -w ready | cut -d ' ' -f1 | grep -v NAME > /var/CKA2022/hk8s-node-ready.txt

$ kubectl config current-context

# k8s 클러스터로 이동
$ kubectl config use-context k8s

# k8s 현재 클러스터의 상태 정보 확인
$ kubectl cluster-info

# gcp node 접속
$ kubectl get nodes
$ gcloud compute ssh --zone asia-northeast3-a <GCP_NODE_NAME>
```