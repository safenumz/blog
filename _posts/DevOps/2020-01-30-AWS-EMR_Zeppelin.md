---
layout: post
title: '[AWS] EMR Cluster 생성 후 Zeppelin 접속'
category: DevOps
tags: [aws, emr, zeppelin]
comments: true
---

# Environment
- MacOS 10.14.6

# 서비스 > 분석 > EMR
- 하나의 EC2 서버가 아니라 하둡, 스파크 클러스터 내에서 데이터 분석 가능

## Amazon EC2 키 페어 생성
- Amazon EC2 콘솔로 이동 (서비스 > EC2 > 네트워크 및 보안 > 키 페어)
- [탐색] 창에서 [키 페어]를 클릭
- [키 페어] 페이지에서 [키 페어 생성]을 클릭
- [키 페어 생성] 대화 상자에서 키 페어의 이름을 입력(예: mykeypair)
- [생성]을 클릭
- 생성된 PEM 파일을 안전한 위치에 저장

### 다운받은 PEM 파일에 권한 부여

~~~shell
$ chmod og-rwx data-engineering.pem
~~~

## 클러스터 생성
- 일반 구성
	- 클러스터 이름: data-engineering
	- 시작 모드(Launch mode): 클러스터

- 소프트웨어 구성(Software configuration)
	- 릴리스(Release): emr 5.28.1
	- 애플리케이션(Application): Spark: Spark 2.4.4 on Hadoop 2.8.5 YARN with Ganglia 3.7.2 and Zeppelin 0.8.2

- 하드웨어 구성(Hardware configuration)
	- 인스턴트 유형(Instance type): c4.large
	- 인스턴트 개수(Number of instance): 3

- 보안 및 액세스(Security and access)
	- EC2 key pair


# EMR > 클러스터 목록 > 요약 > 보안 및 액세스 > 마스터 보안 그룹
## 마스터 노드 및 워커노드의 인바운드 규칙을 각각 편집 (규칙 추가)
- 유형: SSH
- 프로토콜: TCP
- 포트범위: 22
- 소스: 사용자지정 0.0.0.0/0

# EMR > 클러스터 목록 > 요약 > 웹 연결 활성화

~~~shell
$ ssh -i ~/aws/data-engineering.pem -ND 8157 hadoop@ec2-3-133-131-137.us-east-2.compute.amazonaws.com
~~~

## 프록시 관리 도구 설정
- 다음 위치에서 표준 버전의 FoxyProxy를 다운로드하여 설치
[http://foxyproxy.mozdev.org/downloads.html](http://foxyproxy.mozdev.org/downloads.html)
- FoxyProxy를 설치한 후 Chrome을 다시 시작
- 텍스트 편집기를 사용하여 다음 내용이 포함된 foxyproxy-settings.xml이라는 이름의 파일을 생성

### foxyproxy-settings.xml

~~~html
<?xml version="1.0" encoding="UTF-8"?>
<foxyproxy>
    <proxies>
        <proxy name="emr-socks-proxy" id="2322596116" notes="" fromSubscription="false" enabled="true" mode="manual" selectedTabIndex="2" lastresort="false" animatedIcons="true" includeInCycle="true" color="#0055E5" proxyDNS="true" noInternalIPs="false" autoconfMode="pac" clearCacheBeforeUse="false" disableCache="false" clearCookiesBeforeUse="false" rejectCookies="false">
            <matches>
                <match enabled="true" name="*ec2*.amazonaws.com*" pattern="*ec2*.amazonaws.com*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*ec2*.compute*" pattern="*ec2*.compute*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="10.*" pattern="http://10.*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*10*.amazonaws.com*" pattern="*10*.amazonaws.com*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*10*.compute*" pattern="*10*.compute*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*.compute.internal*" pattern="*.compute.internal*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
                <match enabled="true" name="*.ec2.internal*" pattern="*.ec2.internal*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" />
            </matches>
            <manualconf host="localhost" port="8157" socksversion="5" isSocks="true" username="" password="" domain="" />
        </proxy>
    </proxies>
</foxyproxy>
~~~

## FOXY PROXY > Import/Export > Choose File
- foxyproxy-settings.xml를 등록한 후 replace 함
- 크롬 우측 상단 Foxy Proxy 아이콘 클릭 후 use proxy emr socks proxy for all URLs 체크

# EMR > 클러스터 목록 > 요약 > 마스터 퍼블릭 DNS
- 마스터 퍼블릭 DNS 주소 복사 후 크롬 주소창에 넣어 접속

# EMR > 클러스터 목록 > 요약 > Zeppelin
- Zeppelin 활성화 됨
- Zeppelin 접속