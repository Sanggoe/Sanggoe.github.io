---
layout: post
title:  "[Spring] 어노테이션"
subtitle:   "Spring Annotation"
date: 2020-09-22 11:45:51 +0900
categories: study
tags: spring
comments: true
---

### 어노테이션

#### @SpringBootApplication

* 스프링이 실행되기 위한 파일이라는 의미의 어노테이션.
* Main 메소드가 들어있는 코드, 어플리케이션을 나타낸다.

#### @SpringBootTest

* 스프링부트 테스트를 위한 어노테이션이다.
* 스프링 컨테이너와 함께 테스트를 실행하기에, 진짜 스프링을 띄우는 것과 같이 동작한다.

#### @Component

* 컴포넌트 스캐닝을 통해 자동으로 스프링 빈에 등록하기 위해서 사용하는 어노테이션이다.
* Controller, Service, Repository 등의 내부에도 들어가있다.

#### @Controller

* spring container에, 해당 객체를 생성해서 스프링에 넣어두고, 관리하기 위한 어노테이션이다.
* Controller를 통해서 외부 요청을 받아 처리한다.

#### @Service

* spring이 올라올 때, 해당 객체를 spring container에 등록해 관리하기 위한 어노테이션이다.
* Service를 통해서 비스니스 로직을 만들어 수행한다.

#### @Repository

* 마찬가지로 해당 객체를 spring container에 등록해 관리하기 위한 어노테이션이다.
* Repository를 통해서 데이터를 저장한다.

#### @Autowired

* 생성자에 붙여 spring container의 객체와 연결시키기 위한 어노테이션이다.
* spring container에, 자동으로 컨트롤러를 등록하면서 필드로 존재하는 객체의 생성자도 호출한다.
* 그러면 앞서 service로 등록해 spring container 안에 존재하는 객체를 가져다가 연결시켜준다.
* 생성자가 하나이고, 인자로 받는 객체가 Bean으로 등록된 경우, 해당 어노테이션을 생략할 수 있다고 한다!

#### @Configuration

* 스프링에 Bean으로 등록할 객체를 직접 설정하는 Config 클래스에 붙여주는 어노테이션이다.
* 이 어노테이션이 붙어있는 클래스 안에서 @Bean을 이용해 등록해주면 된다.

#### @Bean

* 자바 코드로 스프링 빈을 직접 등록하기 위해 생성자 위에 써주는 어노테이션이다.
* 해당 어노테이션이 있는 클래스에 대해 객체를 만들어 스프링 빈으로 등록해주는 것.
* 그러나 Controller의 경우 자바 코드로 직접 등록하지 않고, 컴포넌트 스캔을 이용한다.

#### @Test

* junit에 속한 것으로, 테스트 하는 메소드라는 의미로 붙여주는 어노테이션이다.

#### @AfterEach

* 각 테스트 메소드가 수행된 후마다 호출되는 메소드라는 의미로 붙여주는 어노테이션이다.

#### @BeforeEach

* 각 테스트 메소드가 수행되기 전에 매번 호출하는 메소드라는 의미로 붙여주는 어노테이션이다.

#### @Entity

* JPA가 관리하는 Entity가 된다는 의미로 붙여주는 어노테이션이다.

#### @Id

* Primary Key로 설정한다는 의미의 어노테이션이다.

#### @GeneratedValue(strategy = GenerationType.IDENTITY)

* Identity : DB가 알아서 ID를 생성해주는 전략 

#### @GetMapping(path)

* 해당 경로로 get 방식으로 요청이 오면 수행할 메소드를 매핑하는 어노테이션이다.

#### @PostMapping(path)

* 해당 경로로 Post 방식으로 요청이 오면 수행할 메소드를 매핑하는 어노테이션이다.

#### @Transactional

* 스프링 AOP 기반으로 만들어진 어노테이션이다.
* 이 어노테이션이 붙어있는 클래스는, 해당 타입의 Proxy가 새로 만들어진다.
* 그러면 기존 코드는 건들지 않고, 기존 기능에 추가 기능까지 수행하는 역할을 한다.

* 만약 이 어노테이션이 테스트 케이스에 붙어있으면, 테스트 시작 전에 트랜잭션을 시작하고 완료 후에 항상 Rollback을 수행해 실제 DB에 반영되지 못하도록 한다.

#### @Commit

* DB에 쿼리 수행 내용을 반영하기 위해 사용하는 어노테이션

#### @Target

* 어노테이션을 정의한 파일(@interface)에 사용하는 어노테이션
* 이 어노테이션을 어디에 쓸 수 있는지 표시하는 용도. 여기선 메소드를 타겟으로 한다.

#### @Retention

* 어노테이션을 정의한 파일(@interface)에 사용하는 어노테이션
* 이 어노테이션 정보를 언제까지 유지할 것인지 표시하는 용도이다.

#### @Aspect

* 어노테이션을 읽어 처리하는 역할을 수행하는 Aspect 클래스라고 명시해주는 어노테이션이다.

#### @Around("@annotation(Annotation_name)")

* AOP를 이용한 Aspect 클래스의 구현을 위해 수행할 메소드에 붙이는 어노테이션이다.
* 정의한 일들을 
* 이 어노테이션이 붙은 메소드는 joinPoint라는 인터페이스 타입의 파라미터를 받을 수 있다.
* 이는 어노테이션이 붙은 타겟, 즉 @Annotation_name 이 붙은 메소드를 받는 것이다.

#### @Order(value)

* Bean의 우선순위가 필요할 때 사용하는 어노테이션
* 객체가 생성되어 Bean으로 등록되는 순서나, 주입받는 우선순위 등을 설정하기 위해 사용한다.
* 주의할 점은, 주입 받는 여러 개의 Bean중 일부만 @Order를 써주게 되면 나머지 Bean은 임의의 순서로 적용된다.
