---
title:  "[Spring Study]  -스프링 DB 기술(3)"
excerpt: "JPA 연결 방식 구현 "

categories: Spring
tags:
  - [Spring, SpringBoot,JPA,DB,Database]
layout : post
toc: true
toc_sticky: true
 
date: 2021-08-09
last_modified_at: 2021-08-09
---

스프링 DB 기술 -JPA 연결 방식 구현

- JPA 연결 방식 구현

  : JPA - 표준 인터페이스, hibernate 등의 구현체로 실제 구현을 하게 됨

  - 환경 설정

    - build.gradle 파일에 jdbc,h2 데이터베이스 관련 라이브러리 추가

      ~~~java
      ```
      implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
      runtimeOnly 'com.h2database:h2'
      ```
      ~~~

    - 스프링 부트 데이터베이스 연결 설정 추가(main/resources/application.properties)

      ~~~java
      ```
      spring.datasource.url=jdbc:h2:tcp://localhost/~/test
      spring.datasource.driver-class-name=org.h2.Driver
      spring.jpa.show-sql=true
      spring.jpa.hibernate.ddl-auto=none
      ``` //빨간줄 뜨면 build.gradle에서 갱신해주기(코끼리 버튼)
      
      ~~~

    - **Entity mapping**(**ORM**:Object Realational Mapping)

      - main/resources/domain/Member.java 

      ~~~java
      ```
      import javax.persistence.*;
      
      @Entity
      public class Member {
      
          @Id @GeneratedValue(strategy= GenerationType.IDENTITY)
          //DB가 알아서 값을 생성해주는 전략 = IDENTITY
          //여기서는 IDENTITY로 id값을 자동으로 할당 받고 있음
          private Long id;
      
        //@Column(name="username") : 이런식으로 db에서 사용하는 이름으로 mapping 시킬 수 있음
          private String name;
      ```
      ~~~

      기존 코드에 에노테이션을 사용해 매핑을 해준다.

      - repository디렉토리에 JpaMemberRepository 생성

        ```java
        package hello.hellospring.repository;
        
        import hello.hellospring.domain.Member;
        
        import javax.persistence.EntityManager;
        import java.util.List;
        import java.util.Optional;
        
        public class JpaMemberRepository implements MemberRepository{
        
            private final EntityManager em; // JPA에서의 동작 관리
        
            public JpaMemberRepository(EntityManager em) {
                this.em = em;
            }
        
            @Override
            public Member save(Member member) {
                return null;
            }
        
            @Override
            public Optional<Member> findById(Long id) {
                return Optional.empty();
            }
        
            @Override
            public Optional<Member> findByName(String name) {
                return Optional.empty();
            }
        
            @Override
            public List<Member> findAll() {
                return null;
            }
        }
        ```

        `MemberRepository`를 implements 해주고, EntityManager 객체를 생성한다.

        Entitiy를 사용하려면 EntityManager가 필요하다.

  - 메서드 작성

    위 `JpaMemberRepository`에서 각 메서드를 작성해보자
    
    ~~~java
    ```save
         @Override
        public Member save(Member member) {
            em.persist(member);
            return member;
        }
    ```
    ~~~
    
      persist : JPA에서 자동으로 insert쿼리를 만들어서 member에 넣어주고, id값까지 생성해준다.
    
    ~~~java
    ```findById
       @Override
        public Optional<Member> findById(Long id) {
            Member member=  em.find(Member.class,id); //select문 대신 JPA로 조회하는 방법
            return Optional.ofNullable(member);
        }
    ```
    ~~~
    
     `Member member=  em.find(Member.class,id);` // JPA에서 select문 대신 조회하는 방법
    
    ~~~java
    ```findByName
        @Override
        public Optional<Member> findByName(String name) {
            List<Member> result =em.createQuery("select m from Member m where m.name = :name",Member.class)
                    .setParameter("name",name)
                    .getResultList();
            return result.stream().findAny();
        }
    ```
    ~~~
    
    `select m from Member m` //member entity 자체를 select(결국 *과 같음)
    
    findAll에서와 같은 방식으로 코드를 짜지 못하는 이유는 찾는 값의 타입(List)과 결과 값의 타입(Member)가 다르기 때문
    
    ~~~java
    ```findAll
        @Override
        public List<Member> findAll() {
            return em.createQuery("select m from Member as m",Member.class)
                    .getResultList();
        }
    ```
    ~~~
    
    
    
  - JPA 사용시 주의 : **Data를 저장하고 변경할때 항상 Transaction이 있어야 함**

    ~~~java
    ```service/MemberService
    @Transactional
    public class MemberService {
        //memberRepository를 생성과 동시에 내부에서 선언해줄 수 있도록 함
        private  final MemberRepository memberRepository;
    
      //  @Autowired
        public MemberService(MemberRepository memberRepository){
            this.memberRepository=memberRepository;
        }
    ```
    ~~~

    위와 같이 class 위에 에노테이션해도 되고, 사용되는 메서드에만 에노테이션해도 사용 가능하다.

  - run을 하기위해서는 `SpringConfig`에서 경로를 설정해줘야한다.

    ~~~java
    ```
    @Configuration
    public class SpringConfig {
    //    private DataSource dataSource;
            private EntityManager em; //이부분
    
        @Autowired
        public SpringConfig(EntityManager em){ //이부분
            this.em=em;
        }
    
        @Bean
        public MemberService memberService(){
            return  new MemberService(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository(){
            //return new MemoryMemberRepository();
            //return new JdbcTemplateMemberRepository(dataSource);
            return new JpaMemberRepository(em); //이부분
        }
    }
    ```
    ~~~

    JPA에서는 경로를 설정할때도 EntityManager를 사용한다.

    

- 실행하기

  ![image-20210809215029622](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809215029622.png)

  test를 해보면 위와 같이  hibernate에서 자동으로 쿼리문을 생성하여 코드를 실행해주는 것을 확인할 수 있다.

  





