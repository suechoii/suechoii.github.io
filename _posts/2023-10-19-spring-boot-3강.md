---
layout: post
title: Spring Boot - 3강
category: [Springboot]
date: 2023-10-19 21:06 +0800
---

**모노리스 아키텍처**

- Monolith 아키텍쳐는 모든 업무 로직을 하나의 애플리케이션 형태로 묶어 서비스 하는 형태를 뜻한다.
  - 특정 기능 하나를 수정하고자 해도 전체가 영향을 받는 그런 형태이다.
  - 개발 사이클이 길어질수 밖에 없고, 기능 추가하는데 시간이 오래걸린다, 확장성 down~
  - 규묘가 커질수록 개발 및 유지보수, 배포에 이르기까지 많은 문제접을 갖는다.
- 위에 문제점을 해결하기 위해 나온 아키텍쳐가 SOA이다.
  - SOA와 마이크로 서비스의 공통점 : 서비스 단위로 구분해서 독립적인 프로세스로 만들어서 독립적인 프로세서의 동작하는 어플리케이션으로 만들어서 서로 커뮤니케이션하는 형태로 하나의 서비스를 완성하는 개념
  - SOA는 SOAP를 사용하고, 마이크로 서비스는 각각의 서비스 간의 커뮤니케이션 하는 방식이 대표적으로 REST API를 이용한 방식을 사용한다.

**마이크롤서비스 아키텍쳐**

- 소규모의 독립적인 구성요소로 구분하여 개발하는 방식
- 하나의 마이크로서비스는 독립적으로 디자인, 개발, 배치, 관리되어야한다.
- 예 : 서버의 vertical scaling은 비용이 기하급수적으로 늘어난다 -> 마이크로 서비스로 나누면 문제 해결
- 장점 :
  - 개별적인 컴포넌트로 스테일 관리가 쉽다.
  - 결함을 고립하여 전체 시스텡의 다운을 방지할수 있다.
  - 독립적인 배포가 가능하다.
  - 기술에 대한 다양성을 가질수 있다.
- 단점 :
  - 서비스간의 강한 일관성 (ACID)을 달성하기 어렵다.
  - 분산 환경이므로 디버깅과 추적이 어렵다.
  - end-to-end 테스트의 필요성이 증가한다.
  - 애플리케이션 개발과 배포방법 (CI/CD)이 중요해진다.

![diagram](/assets/img/micro_table.png)

- API Gateway는 Zuul을 사용한다.

- Spring Cloud Config는 각각의 마이크로 서비스들이 갖는 구성환경을 중앙집중식으로 모아서 깃에서 관리할수 구현할수 있게 해주고, 마이크로 서비스들은 그 서버에 접속해서 본 마이크로 서비스들의 환경을 가지고 가서 환경 설정을 하게 해준다.

**Spring Cloud Config**

- 분산되어 있는 여러 서비스의 설정을 관리할 수 있는 서버와 클라이언트를 제공한다.

1. Config Server 만들기

- pom.xml 생성할때 dependencies에서 Config Server 선택
- in _application.yml_ :

  ```yml
  server:
    port: 9900

  spring:
    cloud:
      config:
        server:
          git:
            uri: https://github.com/suechoii/suepring-config.git
  ```

- in _ConfigServer.java_ :

```java
@EnableConfigServer
```

- Git이랑 연결하기 : https://github.com/suechoii/suepring-config.git used here
  ![diagram](/assets/img/GitFiles.png)

- After running the ConfigServer Application, use a client to check :
  ![diagram](/assets/img/Postman.png)

2. Config Client 만들기

- pom.xml 생성할때 dependencies에서 Config Client, Spring Boot Actuator 선택
  - Actuator 선택함으로서 git의 내용이 변경됐을때 클라이언트에 그 내용이 반영되도록 해준다
- Use of ConfigClientController :

```java
@RestController
public class ConfigClientController {

    @Value("${namoosori.value}")
    private String configStr;

    @GetMapping("/test")
    public String test() {
        return configStr;
    }

}
```

- In _application.yml_ :

```yml
server:
  port: 8088

spring:
  application:
    name: configtest-dev
  config:
    import: optional:configserver:http://localhost:9900
```

- ConfigServer 실행 후, ConfigClient 실행하면 연결이 될것이다. Postman으로 확인 가능하다:
  ![diagram](/assets/img/Postman2.png)

- 만약에 깃의 있는 파일의 컨텐트가 업데이트된다면, actuator을 사용해야 한다. 설정 방법은 :
  - in _application.yml_ :
  ```yml
  management:
    endpoints:
      web:
        exposure:
          include: refresh
  ```
  - ConfigClientController를 @RefreshScope 으로 지정
  - POST http://localhost:8088/actuator/refresh
