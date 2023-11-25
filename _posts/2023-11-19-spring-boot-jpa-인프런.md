---
layout: post
title: "Spring Boot & JPA - 인프런"
category: [Springboot]
date: 2023-11-19 22:48 +0800
---

JPA

- a Java specification for managing relational data in Java applications. It allows us to access and persist data between Java object/ class and relational database. JPA follows Object-Relation Mapping (ORM).
- It also provides a runtime EntityManager API for processing queries and transactions on the objects against the database. It uses a platform-independent object-oriented query language JPQL (Java Persistent Query Language).
- EntityManager를 통한 변경은 항상 transaction에서 이뤄져야한다
- persist > when creating a new entity object, it’s in the transient lifecycle state, so persist can help to attach the entity to a persistence context so that it becomes managed and get persisted in the database
- find > used to look up entities in the data store by the entity’s primary key
  `Customer cust = em.find(Customer.class, custID);`

- Spring bean autowiring. @Autowired annotation can be applied on variables and methods for autowiring byType. We can also use @Autowired annotation on constructor for constructor based spring autowiring. For @Autowired annotation to work, we also need to enable annotation based configuration in spring bean configuration file. Thisallows Spring to resolve and inject collaborating beans into our bean.

- @Transactional means all happens within a transaction, if any one throws an error, there will be rollback . It also performs rollback for Tests -> @Rollback(False) can be used to prevent any rollbacks.

- In application.yml :
  spring.jpa.hibernate.ddl-auto: create
  이 옵션은 애플리케이션 실행 시점에 테이블을 drop 하고, 다시 생성한다.
