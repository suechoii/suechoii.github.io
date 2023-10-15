---
layout: post
title: Spring Boot - 2강
category: [Springboot]
date: 2023-10-15 17:38 +0800
---

**RESTful 아키텍처 스타일**
- REST(REpresentational State Transfer) 
- 제약 조건 :
    - 모든 리소스는 URI로 식별한다. 
        > URI는 어느 위치에 어떤 자료까지 포함하는 데이터를 말하고, URL은 해당 자원이 있는 위치를 말한다
    - 모든 리소스는 다중 표현 (multiple representation) 을 가질수 있다 (e.g. JSON, xml).
    - 모든 리소스는 표준 HTTP 메소드로 접근/변경/생성/삭제 할수 있다. 
    - 서버는 상태 정보를 갖지 않는다 (e.g. 클라이언트의 세션 정보). 
- 성숙도 : 
    - 레벨 0 : 단일 URI와 단일 동사
    - 레벨 1 : 다중 URI 기반 자원과 단일 동사
    - 레벨 2 : 다중 URI 기반 자원과 동사
    - 레벨 3 : HATEOAS

**RESTful API Naming**
- 간결하고 직관적인 기준 URL을 유지하는 것이 중요하다.
    > 기준 URL에는 동사를 두지 않는다. 
    > 명사를 기준으로 하고, 단수와 복수 중에 기본적으로 복수를 기준으로 한다. 
- Spring MVC : 
    - 클라이언트의 모든 요청은 Dispatcher Servlet이라는 Servlet 객체를 통해서 모든 요청을 받는다.
    - Dispatcher Servlet은 HandlerMapping 기법에 따라 여러 종류가 있다. 어떤 컨트롤러에게 사용자의 요청을 위임해야 할지를 판단해서 해당 컨트롤러에게 그 요청을 위임한다. 
    - 기본적으로 사용하는 기법은 @RequestMapping, @PostMapping, @GetMapping과 같은 어노테이션을 사용해 어떤 컨트롤러로 이동할지를 결정해주는 방법이다. 

**RESTful Web Services 구현**
- Lombok (롬복)은 객체를 만들때 기본적으로 사용되는 다양한 메소드들을 어노테이션을 이용해서 자동 생성해준다. 
- Entity package에 다음과 같은 User 클래스를 생성후, 실행하면 {"id":"17de26ec-3b0a-4f2d-97c4-0296867560e3","name":"kim","email":"kim@suepring.io"} 가 출력된다. 
```java
@Getter
@Setter
@ToString
public class User {

    private String id;
    private String name;
    private String email;

    public User(){
        this.id = UUID.randomUUID().toString();
    }

    public User(String name, String email){
        this();
        this.name = name;
        this.email = email;
    }

    public static User sample() {
        return new User("Choi", "sue@suepring.io");
    }

    public static void main(String[] args) {
        User user = new User("kim", "kim@suepring.io");
        System.out.println(new Gson().toJson(user));
    }
}
```