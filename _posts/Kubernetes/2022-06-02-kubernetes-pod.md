---
layout: post
title: '[Kubernetes] Pod'
category: Kuberntetes
tags: [kubernetes, pod]
comments: true
---

# [Pod](https://kubernetes.io/docs/concepts/workloads/pods/#pod-templates)
- Pod란 컨테이너를 표현하는 k8s API의 최소 단위
- Pod에는 하나 또는 여러 개의 컨테이너가 포함될 수 있음

## 명령어로 CLI 모드에서 생성
- nginx 웹 서버 컨테이너를 pod로 동작시키기
    - pod name: web
    - image : nginx:1.14
    - port : 80

```sh
$ kubectl run web --image=nginx:1.14 --port=80
$ kubectl get pod -n devops
$ kubectl delete pod web
```

## Yaml 템플릿으로 생성
- nginx 웹 서버 컨테이너를 pod로 동작시키기
    - pod name: web
    - image : nginx:1.14
    - port : 80

```sh
# --dry-run은 실제 실행하지 않고 잘 동작할 수 있는지 알려줌
$ kubectl run web --image=nginx:1.14 --port=80 --dry-run=client -o yaml > web.yaml
```

```sh
$ vi web.yaml
```

```yaml
apiVersion: v1
kind: Pod metadata:
name: web 
spec:
  containers:
    - image: nginx:1.14
  name: web
  ports:
    - containerPort: 80
```

```sh
$ kubectl apply -f web.yaml
$ kubectl get pod -n devops
$ kubectl delete pod -n devops web
```

# 문제: Pod 생성
-  작업 클러스터 : k8s
- **cka-exam**이라는 namespace를 만들고, **cka-exam** namespace에 아래와 같은 Pod를 생성
    - pod Name: pod-01
    - image: busybox
    - 환경변수 : CERT = "CKA-cert"
    - command: /bin/sh
    - args: -c "while true; do echo $(CERT); sleep 10;done"

```sh
$ kubectl config use-context k8s
$ kubectl create namespace cka-exam $ kubectl get namespaces cka-exam
$ kubectl run pod-01 --image=busybox --dry-run=client -o yaml > 2-1.yaml 
```

```sh
$ vi 2-1.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-01
  namespace: cka-exam
spec: 
  containers:
  - image: busybox
    name: pod-01
    env:
    - name: CERT 
      value: CKA-cert
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo $(CERT); sleep 10;done"]
```

```sh
$ kubectl apply -f 2-1.yaml
$ kubectl get pod -n cka-exam
```

# 문제: pod의 로그 확인해서 결과 추출하기
- 작업 클러스터 : **hk8s**
- Pod **custom-app**의 log를 모니터링하고 **file not found** 메세지를 포함하는 로그 라인인 추출
- 추출된 결과는 **/opt/REPORT/2022/custom-app-log**에 기록

```sh
$ kubectl config use-context hk8s 
$ kubectl get pod custom-app
$ kubectl logs custom-app | grep 'file not found' > /opt/REPORT/2022/custom-app-log
$ cat /opt/REPORT/2022/custom-app-log
```

# Static Pod 만들기
-  API 서버 없이 특정 노드에 있는 **kubelet**에 의해 직접 관리
-  **/etc/kubernetes/manifests/** 디렉토리에 **pod yaml** 파일을 저장 시 적용됨
-  **static pod** 디렉토리 구성

```sh
# 정해진 곳에다 yaml 파일 넣어 놔야 작동
$ cat /var/lib/kubelet/config.yaml

staticPodPath: /etc/kubernetes/manifests
```

- 디렉토리 수정시 kubelet 데몬 재실행 
    - **systemctl restart kubelet**
- **hk8s-w1**에 static Pod 만들기
    - ssh로 **hk8s-w1** 서버에 접속
    - sudo -i로 root 권한 얻기
    - /etc/kubernetes/manifests 디렉토리에 pod yaml파일을 저장하기

```sh
$ kubectl run webserver --image=nginx:1.14 --port=80 --dry-run=client -o yaml > webserver.yaml

$ kubectl describe pod webserver-hk8s-w1

$ rm webserver.yaml # 파일을 지우면 자동으로 파드가 내려감
```

# 문제: static pod 생성
- **hk8s-w1** 노드에 **nginx-static-pod.yaml** 라는 이름의 Static Pod를 생성
    - pod name: nginx-static-pod
    - image: nginx
    - port : 80

```sh
$ ssh hk8s-w1
$ sudo -i
$ grep staticPod /var/lib/kubelet/config.yaml
staticPodPath: /etc/kubernetes/manifests

$ cd /etc/kubernetes/manifests

$ kubectl run nginx-static-pod --image=nginx --port=80 --dry-run=client -o yaml > nginx-static-pod.yaml
```

```sh
$ vi nginx-static-pod.yaml
```

```yaml
apiVersion: v1 
kind: Pod 
metadata:
  name: nginx-static-pod 
spec:
  containers:
  - name: nginx
    image: nginx 
    ports:
    - containerPort: 80
      protocol: TCP
```

```sh
$ exit
$ kubectl get pods
```

# multi-container
- 하나의 Pod에 여러 개의 컨테이너가 포함되어 함께 실행됨

```yaml
apiVersion: v1
kind: Pod 
metadata:
  name: two-containers
spec:
  containers:
  - name: nginx-container 
    image: nginx 
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html
  - name: debian-container 
    image: debian 
    volumeMounts:
    - name: shared-data
      mountPath: /pod-data
    command: ["/bin/sh"]
    args: ["-c", "echo Hello from the debian container > /pod-data/index.html"]
volumes:
- name: shared-data
  emptyDir: {}
```

# 문제: multi container Pod 생성
- 4개의 컨테이너를 동작시키는 eshop-frontend Pod를 생성
    - pod image: nginx, redis, memcached, consul

```sh
$ kubectl run eshop-frontend --image=nginx --dry-run=client -o yaml > multi.yaml
```

```sh
$ vi multi.yaml
```

```yaml
apiVersion: v1
kind: Pod 
metadata:
  name: eshop-frontend
spec:
  containers:
  - image: nginx
    name: nginx 
  - image: redis
    name: redis
  - image: memcached
    name: memcached
  - image: consul
    name: consul
```

```sh
$ kubectl apply -f multi.yaml
```