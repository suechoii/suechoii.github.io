---
layout: post
title: Spring Boot - 1강
category: [Springboot]
date: 2023-10-06 23:32 +0700
---

**Why Spring Boot ?**
- 스프링 부트는 필요한 환경 설정을 최소화해서 비즈니스 로직에 집중할수 있게끔 한다. 
- 내장된 서버로 단독 실행이 가능한 애플리케이션을 개발할수 있다.
- 다양한 스타터를 제공해서 maven/gradle과 같은 빌드 도구의 구성에 대해서 간소화할수 있다. 

**Comparison with Spring**
- Spring (MVC)을 사용할때는 라이브러리에 대한 의존성 관리부터 서버 설정 (e.g. Apache Tomcat, jetty, undertow) +  개발완료후 export/build/release할때 설정을 다 직접해야한다 ; 확장자가 war. 
- Spring Boot는 확장자가 jar -> 자바 어플리케이션 형태로 빌드, 배포가 가능하다. 또한, 서버들이 미리 내장되어있어서 별도의 (초기) 서버 설정 필요가 없다.

**What is Spring Boot?**
- 대표적인 기능들 : (1) Spring Boot Starter, (2) Spring Boot Configuration, (3) Spring Boot Actuator 
- Spring Boot Starter : 어플리케이션의 목적에 맞는 라이브러리들의 의존성을 자동으로 설정해준다.
> 예를 들어, spring-boot-starter-web 종속성을 추가하면 , spring-boot-starter-web이 가지고 있는 spring-web-*.jar와 같은 다른 종속성들은 자동으로 추가되기 때문에 따로 추가하지 않아도 된다. 
- Automatic Configuration : classpath를 이용해서 어플리케이션 기능에 대한 자동 설정을 제공해준다.
> 에를 들어, spring-boot-starter-data-jpa를 종속성에 추가하면 JPA와 관련된 설정을 자동으로 시도한다. 
- Spring Boot Actuator: 어플리케이션 관리와 모니터링을 쉽게 할수 있다.
> endpoint를 설정해서 bean들의 상태나 메모리의 상태 혹은 요청에 대한 상태 등을 추적하거나 관리할수 있다. 

**Creating a Spring Boot Project**
<a>start.spring.io</a> [Spring initializr] 로 Spring Boot Project의 set-up을 할수 있다.

maven프로젝트 간의 상속 구조가 가능해서, 아래와 같이 pom.xml에 설정을 한다면 해당 groupId와 artifactId를 갖고 있는 설정을 그대로 갖고 오게된다.
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.1.4</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>
```

간단한 Application / Controller 클래스 예시
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello Spring Boot";
    }
}
```

Spring Configuration 할때 두가지 방법이 있다 : 
1. 기존 xml을 사용하는 방법, 
2. Java를 사용해 annotation을 주고 특정 bean을 등록하는 방법. 해당 bean은 설정을 위해 사용되는 클래스
- Spring Boot는 @Configuration 이라는 annotation을 갖고 있는 다양한 기본 설정 클래스들을 갖고있다 --> @SpringBootConfiguration, @EnableAutoConfiguration, @ComponentScan
> ComponentScan이란? Spring bean 객체들을 찾는데 사용된다 - 특정 패키지로부터 그런 해당 annotation이 붙은 클래스들을 찾아서 bean을 등록해준다.
> SpringBootApplication annotation을 갖고 있는 클래스는 항상 root directory에 넣어주기! 그래야지 아래로 내려가면서 component scan이 가는하기 때문이다. 