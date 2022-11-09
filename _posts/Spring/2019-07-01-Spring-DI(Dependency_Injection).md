---
layout: post
title: '[Spring] DI(Dependency Injection)'
category: Spring 
tags: [Spring, DI]
comments: true
---

# Spring DI(Dependency Injection)

## 기존 개념과 코드

~~~java
// 한국어 메시지를 출력하려면?
MessageBean bean = new MessageBeanKoImpl();
bean.sayHello("홍길동");
~~~

~~~java
// 한국어 메시지를 영어 메시지로 변경하려면?
MessageBean bean = new MessageBeanEnImpl();
bean.sayHello("Jone");
~~~

## IoC (Inversion of Control: 역행제어)
> IoC(제어권의 역전)이란, 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미한다.
- 컴포넌트 의존관계 결정 (component dependency resolution), 설정(configuration) 및 생명주기(lifecycle)를 해결하기 위한 디자인 패턴(Design Pattern)

## IoC 컨테이너
- 스프링 프레임워크도 객체에 대한 생성 및 생명주기를 관리할 수 있는 기능을 제공하고 있음. 즉 IoC 컨테이너 기능을 제공한다.
	- IoC 컨테이너는 객체의 생성을 책임지고, 의존성을 관리한다.
	- POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가진다.
	- 개발자들이 직접 POJO를 생성할 수 있지만 컨테이너에게 맡긴다.

## IoC 분류
- IoC : Inversion of Control
- DI : Dependency Injection
- DL : Dependency Lookup

~~~shell
IoC -- DL
    |
    -- DI -- Setter Injection
          |
          -- Constructor Injection
          |
          -- Method Injection
~~~

## DL과 DI
- DL (Dependency Lookup) 의존성 검색 : 저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup 하는 것
- DI (Dependency Injection) 의존성 주입 : 각 클래스간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동을 연결해주는 것
- DL 사용시 컨테이너 종속성이 증가하여, 주로 DI를 사용함

## DI 개요
> 각 클래스간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말함
- 개발자들은 단지 빈 설정파일에서 의존관계가 필요하다는 정보를 추가하면 된다.
- 객체 레퍼런스를 컨테이너로부터 주입 받아서, 실행 시에 동적으로 의존관계가 생성된다

### DI (Dependency Injection) 장점
- 코드가 단순해진다.
- 컴포넌트 간의 결합도가 제거된다.

## 관련용어
### 빈(Bean)
- 스프링이 IoC 방식으로 관리하는 객체라는 뜻
- 스프링이 직접 생성과 제어를 담당하는 객체를 Bean이라고 부름

### 빈 팩토리(BeanFactory)
- 스프링의 IoC를 담당하는 핵심 컴테이너를 가리킴
- Bean을 등록, 생성, 조회, 반환하는 기능을 담당함
- 이 BeanFactory를 바로 사용하지 않고 이를 확장한 ApplicationContext를 주로 이용함

### 애플리케이션 컨텍스트(Application Context)
- BeanFactory를 확장한 IoC 컨테이너
- Bean을 등록하고 관리하는 기능은 BeanFactory와 동일하지만 스프링이 제공하는 각종 부가 서비스를 추가로 제공함
- 스프링에서는 ApplicationContext를 BeanFactory 보다 더 많이 사용함

## 빈 설정방법
### XML 기반 설정 방식
- 스프링 1.0
- xml 파일에 <bean> 요소로 빈을 정의한다.
- <constructor-arg>나 <property> 요소로 의존성을 주입한다.

### Annotation 기반 설정 방식
- 스프링 2.5
- @Component 같은 어노테이션이 부여된 클래스를 탐색하여 DI 컨테이너에 빈을 등록한다.

### Java 기반 설정 방식
- 스피링 3.0
- 자바 클래스에 @Configuration을 메소드에 @Bean 어노테이션을 사용해 빈을 정의한다.

# 빈 설정 
## 1. XML 기반
### (1) Spring Bean Configuration File 생성
- src 폴더에서 오른쪽 마우스 > other > Spring 선택
- 파일명: applicationContext

### (2) Spring Bean Configuration File(applicationContext.xml)에 빈 정의

~~~html
<!-- applicationContext.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<!-- DI : 빈 정의 -->
<bean id="ko" class="ex1_xml1_scope.MessageBeanKoImpl"></bean>
<bean id="en" class="ex1_xml1_scope.MessageBeanEnImpl"></bean>
</beans>
~~~

### MainApp 클래스에서

~~~java
// 1. 스프링의 설정파일을 연결
ApplicationContext context = new ClassPathXmlApplicationContext("ex1_xml1_scope/applicationContext.xml");

// 2. DI 컨테이너에서 빈 가져오기
MessageBean bean = context.getBean("ko", MessageBean.class);

// 자바소스에서 new 코딩을 없애고 두 클래스간의 결합도를 낮춰서
// 한쪽 클래스 변경에 의한 새로 컴파일/복사/실행을 안 해도 될 수 있음
~~~

### [참고] 스프링 설정 파일 연결

~~~java
ApplicationContext ctx = new ClassPathXmlApplicationContext("exercise/beans.xml");

ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);

ApplicationContext ctx = new FileSystemXmlApplicationContext("src/exercise/beans.xml)
~~~

### [참고] BeanFactory와 ApplicationContext

<img src="https://hhqa5a.bn.files.1drv.com/y4mlRU1bltHnDS2tgwePpp22chOUwr3XX-oNWdXI7gZfWz9X_s3ljIvM0mtpId0-jEid-P9hh9SAdTI6SJppPYtSYiiNjeuwb7xB0Ux7LO0JLDbd33PjVZnzo8nOwCCgVJu2-kmkH0YjeW0Z5GdF6CHvCP14KepZkyWv3MGjy-CKf_DtPyW-gOBt_rlHa-zFaWuUBeVACGyeeiu6AJv4bo0Fw?width=1124&height=620&cropmode=none"  />

## (3) 생성자 주입 - 멤버 변수가 객체일 때

~~~java
public class MemberDao {
	private MemberBean member;

	// 생성자
	public MemberDAO() {
	}

	public MemberDAO(MemberBean member) {
		this.member = member;
	}
}
~~~

~~~html
<bean id="member" class="ex1_xml2_ref.MemberBean"></bean>
<bean id="doo1" class="ex1_xml2_ref.MemberDAO">
	<constructor-arg ref="member"></constructor-arg>
</bean>
~~~

## 2. Annotation 기반
### (1) Component Scan
- Annotation이 붙은 클래스를 탐색하여 DI 컨테이너에 자동으로 등록

~~~java
@Component
public class MemberBean {
	private String name;
	private int age;
	private String message;
}
~~~

~~~java
@Component
public class MemberDAO {
	private MemberBean member;
}
~~~

### (2) Auto Wiring
- DB 컨테이너가 자동으로 필요로 하는 의존 컴포넌트를 주입

~~~java
@Component
public class MemberDAO {
	// 필드 기반 의존성 주입 법칙
	@Autowired
	private MemberBean member;
}
~~~

### (2) xml 기반 양식으로 컴포넌트 스캔 범위 설정
- applicationContext.xml

~~~html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/context "

	<context:component-scan bese-package="ex2_annotation"/>
</beans>
~~~

## 3. Java 기반

### (1) 자바 코드로빈을 설정 (Java Configuration Class)
- AppConfig.java

~~~java
@Configuration
@ComponentScan("ex3_javabased")
public class AppConfig {
	@Bean
	MemberBean memberBean() {
		return new MemberBean("홍자", 33, "오늘도 행복");
	}

	@Bean
	MemberDAO memberDAO() {
		return new MemberDAO(memberBean());
	}
}
~~~

## (2) 스프링 설정 파일 연결
- MainApp.java

~~~java
public static void main(String[] args) {
	ApplicationContext context = 
		new AnnotationConfigApplicationContext(AppConfig.class);

	MemberDAO dao = context.getBean("memberDAO", MemberDAO.class);
	dao.insert();
}
~~~

## 의존성 주입 어노테이션

어노테이이션    | 설명
:-----------:|:-------------------------------:
@Autowired   | 스프링 제공 
/            | 생성자, 메소드, 변수 위에 모두 사용 가능하지만 주로 변수 위에 설정하여 해당 타입의 객체를 자동으로 할당
@Qualifier   | 스프링 제공
/            | 특정 객체의 이름으로 의존성 주입할 때 사용
@Resource    | javax.annotation.Resource
/            | @Autowired와 @Qualifier 기능을 결합함 역할
@Inject      | javax.inject.Inject
/            | @Autowired와 동일한 기능

- 스프링 프레임워크에는 클래스를 분류하기 위해 @Component를 상속한 어노테이션을 제공

어노테이이션    | 클래스            | 설명
:-----------:|:---------------:|---------------------:
@Service     | XXXServiceImpl  | 비즈니스 로직을 처리하는 Service 클래스
@Repository  | XXXDAO          | 데이터베이스 연동을 처리하는 DAO 클래스
@Controller  | XXXController   | 사용자 요청을 제어하는 Controller 클래스

## 연습 1

<img src="https://hhqh5a.bn.files.1drv.com/y4m-LGYSTufz3f6GwO6h5_o0k_5MUdBqxTVEaZXQyuFf0U_rqFLYz1mnYDzYH7uZW4CuwWGJ3WnWoIFGAXAbLkF_efIMUArGb8ZrrxLIJDVy6bcjwBT3bvRe0RU7VHTgTHL_ZhKZFuatTirPXS4INkJk8AJAJgBDbj4EMQcsnSOMzuNZ3bZa7gsd2pT-Z9_4anYsxtoBIyiahLbFqsfYpse3Q?width=1444&height=914&cropmode=none" />


## 연습 2

<img src="https://hhqg5a.bn.files.1drv.com/y4mCwtzohGOLkrQVe_mlT4_AN-9t7YeCauXRPfHXbHswTkIr858EFIEgWepHiFBs4EM_JjFqHcOVX7xAS-E1HvblWMIOtvVZjTNbkfItjLdL5VQDtoa9lyE91fo5jo4c9QFomTcxTwj2JMebz6nQ2g6xT1wAPJr9LCuEbpOxbTz1KBSWJe5pme5h1pekvLn-Xse3yPCfREAt6zjoWQ16FclHg?width=1584&height=724&cropmode=none"  />






