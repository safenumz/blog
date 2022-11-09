---
layout: post
title: '[Spring] Spring 프레임워크'
category: Spring
tags: [java, spring, 스프링, 프레임워크]
comments: true
---

# 프레임워크
- 프레임워크는 애플리케이션들의 최소한의 공통점을 찾아 하부 구조를 제공함으로써 개발자들로 하여금 시스템의 하부 구조를 구현하는데 들어가는 노력을 절감하게 하는 것

## 프레임워크 장점
- 비기능적인 요소들을 초기 개발 단계마다 구현해야 하는 불합리함을 극복해준다.
- 기능적인(Functional) 요구사항에 집중할 수 있도록 해준다.
- 디자인 패턴과 마찬가자로 반복적으로 발견되는 문제를 해결하기 위한 특화된 Solution을 제공한다.

## 프레임워크 vs 디자인 패턴
- 디자인패턴: 애플리케이션을 설계할 때 필요한 구조적인 가이드라인이 되어 줄 수는 있지만 구체적으로 구현된 기반코드를 제공하지는 않는다.
- 프레임워크: 디자인 패턴과 함께 패턴이 적용된 기반 클래스 라이브러리를 제공해서 프레임워크를 사용하는 구조적인 틀과 구현코드를 함께 제공한다.

## 프레임워크 vs 클래스라이브러리
- 클래스 라이브러리: 개발자가 만든 클래스에서 직접 호출하여 사용하므로 실행의 흐름에 대한 제어를 개발자의 코드가 관장하고 있다.
- 프레임워크: 프레임워크에서 개발자가 만든 클래스를 호출하여 실행의 흐름에 대한 제어를 담당한다.

# Spring 프레임워크
## 스프링이란
- 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크
- 애플리케이션 프레임워크: 애플리케이션 전 영역을 관통하는 일관된 프로그래밍 모델과 핵심 기술을 바타응로 각 분야의 특정에 맞는 기능을 제공
- 경량급: 스프링은 단순한 개발 툴과 기본적인 개발환경으로, 필요하게 등장하던 프레임워크나 서버환경에 의존적인 부분을 제거
- 개발을 편하게 해주는: 스프링의 기본설정과 적용기술만 잘 선택하고 준비해두면, 개발할 때 스프링과 관련된 코드나 API에 대해 거의 신경 쓸 일이 없다.

## 스프링 프레임워크의 발전
### 1.x
- EJB의 대안으로 소개
- IoC(DI), AOP, XML 기반의 빈 정의

### 2.x
- 스프링 시큐리티
- DI와 MVC를 Annotation 방식 설정

### 3.x
- RESTful 프레임워크 사용

### 4.x
- 웹소켓

### 5.x
- 리액티브 프로그래밍

## 스프링 주요 프로젝트 및 라이브러리

모듈            | 설명
:-------------:|:-----------------:
spring-beans   | 스프링 컨테이너를 이용해서 객체를 생성하는 기본 기능 제공
spring-context | 객체 생성, 라이프 사이클 처리, 스키마 확장 등의 기능을 제공
spring-aop     | AOP 기능을 제공
spring-web     | REST Client, 데이터 변환 처리, 서블릿 필터, 파일 업로드 지원 등 웹 개발에 필요한 기반 기능 제공
spring-webmvc  | 스프링 기반의 MVC 프레임워크, 웹 어플리케이션을 개발하는데 필요한 Controller, View 구현을 제공
spring-oxm     | XML과 자바 객체 간의 매핑을 처리하기 위해 API 제공
spring-tx      | 트랜젝션 처리를 위한 추상 레이어를 제공
spring-jdbc    | JDBC 프로그래밍을 보다 쉽게 할 수 있는 템플릿을 제공
spring-orm     | 하이버네이터, JPA, MyBatis 등과의 연동을 지원
spring-jms     | JMS 서버와 메시지를 쉽게 주고 받을 수 있도록 하기 위한 템플릿, 애노테이션 등을 제공
spring-context-support | 스케쥴링, 메일 발송, 캐시 연동, 벨로시티 등 부가 가능을 제공

## 스프링 프레임워크
![스프링프레임워크](https://6kysqq.bn.files.1drv.com/y4mW-VQlytHNd06_hlYWEwSX9moE4ptX4Vs0UQTggGCb3reYJ6T9XFrcLVB9T5Zi8A0JSy-BuoBZd4ARWAcuZU6Cs8JjLznjPM8Pokrav_j45GL7spM8ZGfKTtM_2hr3xqbtitDQ5tb_dobzd10VzCnIH6zcGEj6WvUTKb-aH9Y1bf00wzGk40ba3yOPv5loAk0ZASfZDFpzj0_Mkm-PYOiTQ?width=581&height=448&cropmode=none)