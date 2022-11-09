---
layout: post
title: '[AWS] awscli 설정'
category: DevOps
tags: [aws]
comments: true
---

# Environment
- MacOS 10.14.6

# AWS 사용자 추가
[https://console.aws.amazon.com/iam/home?region=ap-northeast-2#/home](https://console.aws.amazon.com/iam/home?region=ap-northeast-2#/home)

## 서비스 > IAM > 사용자 > 사용자 추가

### 사용자 세부 정보 설정
- 사용자 이름 등록

### AWS 액세스 유형 선택
- 액세스 유형: 프로그래밍 방식 액세스 체크

### 권한 설정 > 기존 정책 직접 연결
- AdministratorAccess 체크

### 태그 추가(선택사항)
- 공란으로 남겨 둠

사용자 만들기 완료를 누르면 사용자, 액세스키 ID, 비밀 액세스키가 나타난다.

# awscli 설치
## Python Packaging Authority에서 제공하는 스크립트를 사용하여 pip를 설치

~~~shell
$ curl -O https://bootstrap.pypa.io/get-pip.py
$ python3 get-pip.py --user
~~~

## pip를 사용하여 AWS CLI를 설치

~~~shell
$ pip3 install awscli --upgrade --user
~~~

## AWS CLI가 올바르게 설치되었는지 확인

~~~shell
$ aws --version
~~~

<pre>
aws-cli/1.17.9 Python/3.7.3 Darwin/18.7.0 botocore/1.14.9
</pre>

## 구성하기
- aws configure 명령을 실행하여  AWS CLI 설정할 수 있음

~~~shell
$ aws configure
AWS Access Key ID [None]: <액세스키 ID>
AWS Secret Access Key [None]: <비밀 액세스키>
Default region name [None]: <리전, ap-northeast-2>
Default output format [None]: 
~~~
