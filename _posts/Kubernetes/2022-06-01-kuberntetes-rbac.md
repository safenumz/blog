---
layout: post
title: '[Kubernetes] RBAC 인증'
category: Kuberntetes
tags: [kubernetes, rbac]
comments: true
---

# API 인증: RBAC 인증
- API 서버에 접근하기 위해서는 인증 작업 필요
- Role-based access control(RBAC, 역할기반 액세스 제어) 사용자의 역할에 따라 리소스에 대한 접근 권한를 가짐
- User: 클러스터 외부에서 쿠버네티스를 조작하는 사용자 인증
- API 요청 &rarr; Authentication &rarr; Athorization &rarr; Addmission Control &rarr; 요청 승인

```sh
$ cat .kube/config
```

```yaml
contexts:
  - context:
    cluster: kubernetes
    user: kubernetes-admin
    name: kubernetes-admin@kubernetes

..

users:
  - name: kubernetes-admin
    user:
      client-certificate-data: LS0tLS1CRU--인증서
```

- ServiceAccount: Pod가 쿠버네티스 API를 다룰때 사용하는 계정 

```sh
$ kubectl get pods eshop-cart-app -o yaml | grep -i serviceaccount
$ kubectl get secrets default-token-gvdn7
```

# Role & RoleBinding를 이용한 권한 제어
- 특정 유저나 ServiceAccount가 접근하려는 API에 접근 권한을 설정
- 권한 있는 User만 접근하도록 허용
- 권한 제어
- Role
    - 어떤 API를 이용할 수 있는지의 정의
    - 쿠버네티스의 사용권한을 정의
    - 지정된 네임스페이스에서만 유효
- RoleBinding
    - 사용자/그룹 또는 Service Account와 role을 연결

# Role
- 어떤 API를 사용할 수 있는지 권한 정의
- 지정된 네임스페이스에서만 유효

| verb | meaning |
|:---:|:---:|
| create | 새로운 리소스 생성 |
| get | 개별 리소스 조회 |
| list | 여러 건의 리소스 조회 |
| update | 기존 리소스 내용 전체 업데이트 |
| patch | 기존 리소스 중 일부 내용 변경 |
| delete | 개별 리소스 삭제 |
| deletecollection | 여러 리소스 삭제 |

## Role 예제: default 네임스페이스의 Pod에 대한 get, watch, list할 수 있는 권한

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
  rules:
  - apiGroups: [""]
    verbs: ["get", "list", "watch"]
```

- pod는 코어 API이기 때문에 apiGroups을 따로 지정하지 않음
- 만약 리소스가 job이라면 apiGroups에 "batch"를 지정
- verbs에는 단어 그대로 나열된 API 리소스에 허용할 기능을 나열함

# RoleBinding
- 사용자/그룹 또는 Service Account와 role을 연결

## RoleBinding 예제: default 네임스페이스에서 유저 jane에게 pod-reader의 role을 할당

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBiding
metadata:
  name: read-pods
  namespace: default
subjects:
  - kind: User
    name: jane
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

# ClusterRole
- 어떤 API를 사용할 수 있는지 권한 정의
- 클러스터 전체(전체 네임스페이스)에서 유효

## ClusterRole 예제: 전체 네임스페이스의 Secret에 대한 get, watch, list할 수 있는 권한

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metatdata:
  name: secret-reader
rules:
  - apiGroups: [""]
    resources: ["secrets"]
    verbs: ["get", "watch", "list"]
```

# ClusterRoleBinding
- 사용자/그룹 또는 Service Account와 role을 연결

## Cluster/RoleBinding 예제: manager 그룹의 모든 사용자가 모든 네임스페이스의 secret을 읽을 수 있도록 구성

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
  - kind: Group
    name: manager
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

# 문제: ServiceAccount, Role, RoleBinding
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- 작업 클러스터 : k8s
- 애플리케이션 운영중 특정 namespace의 Pod들을 모니터할 수 있는 서비스가 요청되었음
- api-access 네임스페이스의 모든 pod를 view할 수 있도록 다음의 작업을 진행
    - api-access라는 새로운 namespace에 pod-viewer라는 이름의 ServiceAccount를 만듬
    - podreader-role이라는 이름의 Role과 podreader-rolebinding이라는 이름의 RoleBinding을 만듬
    - 앞서 생성한 ServiceAccount를 API resource Pod에 대하여 watch,list,get을 허용하도록 매핑

```sh
$ kubectl config current-context

# namespace 생성
$ kubectl create namespace api-access
$ kubectl create serviceaccount pod-viewer -n api-access
$ kubectl get serviceaccounts --namespace api-access
```

```sh
# role 생성
$ kubectl create role podreader-role --verb=watch --verb=list --verb=get --resource=pods --namespace api-access

$ kubectl get role --namespace=api-access
$ kubectl describe role --namespace=api-access podreader-role
```

- role 생성은 yaml 파일로도 만들 수 있음

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: podreader-role
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

```sh
# rolebinding 생성
$ kubectl create rolebiding podreader-rolebinding --role=podreader-role --serviceaccount=api-access:pod-viewer -n api-access

$ kubectl describe -n api-access rolebindings.rbac.authorization.k8s.io podreader-rolebinding
```

# 문제: ServiceAccount, CluserRole, ClusterRoleBinding
- 작업 클러스터: k8s
- 작업 Context에서 애플리케이션 배포를 위해 새로운 ClusterRole을 생성하고 특정 namespace의
ServiceAccount를 바인드
    - 다음의 resource type에서만 Create가 허용된 ClusterRole deployment-clusterrole을 생성
        - Resource Type: Deployment StatefulSet DaemonSet
    - 미리 생성된 namespace api-access 에 cicd-token이라는 새로운 ServiceAccount를 만듬
    - ClusterRole deployment-clusterrole을 namespace api-access로 제한된 새 ServiceAccount cicd-token에 바인딩

```sh
# role 생성: Deployment StatefulSet DaemonSet
$ kubectl create clusterrole deployment-clusterrole --verb=create --resource=deployment,statefulset,daemonset
$ kubectl get clusterrole deployment-clusterrole

# serviceaccount 생성
$ kubectl create serviceaccount cicd-token --namespace=api-access 
$ kubectl get serviceaccounts --namespace api-access

# cluster role binding
$ kubectl create clusterrolebinding deployment-clusterrolebinding --clusterrole=deployment-clusterrole --serviceaccount=api-access:cide-token --namespace=api-access
$ kubectl describe clusterrolebindings deployment-clusterrolebinding
```

 # Role & RoleBinding를 이용한 권한 제어
 - 특정 유저나 ServiceAccount가 접근하려는 API에 접근 권한을 설정
 - 권한 있는 User만 접근하도록 허용
 - 권한 제어
 - Role
    - 어떤 API를 이용할 수 있을지의 정의
    - 쿠버네티스의 사용권한을 정의
    - 지정된 네임스페이스에서만 유효
- RoleBinding
    - 사용자/그룹 또는 Service Account와 role을 연결

# 문제: User, ClusterRole, ClusterRoleBinding
- 작업 클러스터 : k8s
- CSR(Certificate Signing Request)를 통해 app-manager 인증서를 발급받은 user app-manager에게 cluster 내 모든 namespace의 deployment, pod, service 리소스를 create, list, get, update, delete 할 수 있는 권한을 할당
    - user name : app-manager
    - certificate name: app-manager
    - clusterRole name : app-access
    - clusterRoleBinding name: app-access-binding
- 인증서 생성: https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user

```sh
# 키 생성
$ openssl genrsa -out app-manager.key 2048

$ openssl req -new -key app-manager.key -out app-manager.csr -subj "/CN=app-manager"
$ cat app-manager.csr | base64 | tr -d "\n"
```

```sh
$ vi app-manager.yaml
```

```yaml
# app-manager.yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: app-manager
spec:
  request: <KEY>
  signerName: kubernetes.io/kube-apiserver-client
#   expirationSeconds: 86400  # one day
  usages:
  - client auth
```

```sh
$ kubectl apply -f app-manager.yaml
```

```sh
# Pending 상태: 승인요청 해야 함
$ kubectl get csr app-manager -o jsonpath='{.status.certificate}'| base64 - d > app-manager.crt

# 승인 요청
$ kubectl certificate approve app-manager

# 확인
$ kubectl get csr
$ kubectl get csr/app-manager -o yaml

# clusterrole 생성
$ kubectl create clusterrole app-access --verb=create,list,get,update,delete --resource=deployment,pod,service

# cluster role binding 생성
$ kubectl create clusterrolebinding app-access-binding --clusterrole=app-access --user=app-manager

# Add to kubeconfig
$ kubectl config set-credentials app-manager --client-key=app-manager.key --client-certificate=app-manager.crt --embed-certs=true

$ kubectl config set-context app-manager --cluster=kubernetes --user=app-manager
$ kubectl config use-context app-manager
# $ kubectl config use-context kubernetes-admin@kubernetes
```