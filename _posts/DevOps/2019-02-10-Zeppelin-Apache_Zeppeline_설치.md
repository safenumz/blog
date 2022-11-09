---
layout: post
title: '[Zeppelin] Zeppeline 설치'
category: DevOps
tags: [zeppeline, spark]
comments: true
---

# Apache Zeppeline 설치

## Zeppelin 디렉토리 구조
- zeppelin
  - conf : 설정파일들이 저장되어 있음
    - zeppelin-env.cmd.template : 윈도우 유저, 원하는 부분을 주석 풀어서 사용
    - zeppelin-env.sh.template : 리눅스 또는 맥 유저
  - bin
    - zeppelin.cmd


## Zeppelin 실행
- 윈도우즈는 bin 폴더 내에 zeppelin.cmd를 실행(맥이나 리눅스는 zeppelin-demon.sh start)하고
- http://localhost:8080 에 접속하면 zeppelin notebook으로 진입할 수 있다


### 만약 8080 포트가 이미 점유 되어 있다면 포트를 변경할 수 있다
- conf 폴더로 진입

```
// zeppelin-site.xml.template를 복제 후 tempate 확장자 제거
$ cp zeppelin-site.xml.template zeppelin-site.xml

// zeppelin-site.xml를 열어 8080 포트를 다른 포트로 변경할 수 있다
$ vi zeppelin-site.xml
```
