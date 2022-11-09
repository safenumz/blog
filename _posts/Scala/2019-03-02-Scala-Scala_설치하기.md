---
layout: post
title: '[Scala] Scala 설치하기'
category: Scala
tags: [scala]
comments: true
---

# Scala 설치

## Scala 설치 순서
### JDK 설치
- JDK 1.6이상 버전이 설치되어야 함

### Scala 설치
- 설치 파일: [http://www.scala-lang.org/download/](http://www.scala-lang.org/download/)

### scala와 scalac 경로 설정
- OS 환경에 따라 두 실행파일의 경로 등록


```scala  
// tab 대신 space bar 두번으로 대체
// MyFirstScala.scala로 저장

object MyFirstScala {
  def main(args: Array[String]){
    println("Hello Scala")
  }
}
```

```
MyFirstScala.scala가 저장된 디렉토리로 이동하여 아래 명령어를 실행하면 "Hello Scala"를 출력할 수 있다

$ scalac MyFirstScala.scala
$ scala -classpath . MyFirstScala
```

## 스칼라를 위한 프로그래밍 도구
### Scala Interpreter
- 파이썬 등과 같이 인터프리터를 제공함
- 즉각적인 피드백을 주기 때문에 작은 모듈 작성이나 분석용으로 활용하기 좋음
- RELP(read-eval-print loop)
- 엄밀히 말하면 인터프리터가 아니라 내부적으로 코드를 빠르게 컴파일한 후 바이트코드를 JVM(자바 가상 머신)에서 실행

### 외부도구
- IntelliJ
- TypeSafe Activator
- Scala IDE
- SBT

## Intellij 실습
### Intellij 설치
- [https://www.jetbrains.com/idea/download/](https://www.jetbrains.com/idea/download/)

### 장점
- code completion
- interpolitate String
- inpection
- SBT project auto-import

### 실습
- IntelliJ를 설치하고 Project를 생성
- scala Worksheet 생성하여 실습 진행
- src 밑에 Scala class 파일 object 생성(MainClass.scala)


```scala
object MainClass {
  def main(args: Array[String]) {
    println("Hello IntelliJ IDEA")
  }
}
```


```scala
// 1부터 10까지 range
1 to 10

// 리스트 생성
List(1, 2, 3)

// 1. 을 입력하고 ctrl+space를 누르면 호출할 수 있는 메서드를 네비게이션 할 수 있음
````
