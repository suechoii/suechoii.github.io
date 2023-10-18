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
- Client - (req)> Dispatcher Servlet ->  Controller -> Service -> Store -> DB -> Store -> Service -> Controller -(resp)> Dispatcher Servlet
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
- Bean 객체를 등록하기 위해서는 @Repository로 지정해야함. 
- 아래와 같이 @Autowired을 사용해서 지정해주면 UserStore라는 인터페이스를 implement하고 있는 구현 클래스를 찾고, 그 클래스가 userStore에 주입이 된다 => <u>Dependency Injection</u>
```java
@Autowired
private UserStore userStore;
```
> 생성자를 사용한는 alternative method도 있다. 이때, 아래와 같이 생성자를 생성해도 되고, lombok에 제공해주는 @RequiredArgsConstructor를 사용해도 된다 - 대신 userstore을 final로!

    ```java
    private UserStore userStore;
        
    public UserServiceLogic(UserStore userStore){
        this.userStore = userStore;
    }
    ```

- Controller 예시
```java
@PostMapping("/users")
public String register(@RequestBody User newUser){
    return userService.register(newUser);
}
```
-> 위에 PostMapping이 잘 되는지 확인은 manually Postman이나 insomnia 사용해서 할수 있다. 

**JUnit을 사용한 Unit Test**
- 클래스 네이밍은 테스트 하려는 클래스 이름 뒤에 Test만 붙이면 된다 , e.g. UserServiceLogicTest 
- 클래스에 @SpringBootTest 어노테이션 지정해준다.
- 단위테스트에서는 DI할수 있는 방법이 단 한가지 밖에 없다 --> @Autowired
- 각 테스트들은 @Test 어노테이션 지정해준다.
- 테스트하고자 하는 테스트 이름에 option + enter 누르면 테스트 자동 생성 가능하다..!
* 이때, 테스트 package이름도 main package이름이랑 동일하도록 주의 (오타땜에 헤맸다...)
```java
@SpringBootTest
public class UserServiceLogicTest {

    @Autowired
    private UserService userService;

    @Test
    public void registerTest(){
        User sample = User.sample();
        assertThat(this.userService.register(sample)).isEqualTo(sample.getId());
        assertThat(this.userService.findAll().size()).isEqualTo(1);
    }
} 
```
- @BeforeEach, @AfterEach는 각각 테스트 전, 후에 항상 실행된다. 
- @RestController는, request + response가 JSON 형태이다. 
- UserController 클래스 테스트 할때는 MockMvc 클래스 사용이 필요하다.
    - MockMvc란 임의적으로 Post request등을 보낼수 있게끔 해주는 클래스이다. 
    ```java
    @Autowired
    private MockMvc mockMvc;
    ```
    - 이때, mockMvc를 사용하려면 @SpringBootTest 와 같이 @AutoConfigureMockMvc를 지정해야한다. 
    ```java
    @SpringBootTest
    @AutoConfigureMockMvc
    ```
    - mockMvc를 사용해 postMapping 테스트 하는 예시 : 
    ```java
    @Test
    void register() throws Exception {
        User sample = User.sample();
        String content = objectMapper.writeValueAsString(sample);
        mockMvc.perform(post("/users")
                .content(content)
                .contentType(MediaType.APPLICATION_JSON)
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().string(sample.getId()))
                .andDo(print());
    }
    ```
    - 이때, post나 status같은 메서드는 직접 manually import static method를 해야 한다. 
    
    > import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post; 

    > import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print; 

    > import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content; 

    > import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;