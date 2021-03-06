---
title:  "[Spring Study]  - 스프링 데이터 JPA"
excerpt: "스프링 데이터 JPA"

categories: Spring
tags:
  - [Spring, SpringBoot,SpringDataJPA]
layout : post
toc: true
toc_sticky: true
 
date: 2021-08-14
last_modified_at: 2021-08-14
---

- **스프링 데이터 JPA**

  : 스프링 부트와 JPA만 사용해도 개발 생산성이 정말 많이 증가하고, 개발해야할 코드들도 확연히 줄여든다.

  여기에서 **스프링 데이터 JPA**를 사용하면, 기존의 한계를 넘어 **리포지토리에 구현 클래스 없이 인터페이스 만**으로 개발을 완료할 수 있다. 그리고 반복 개발해온 **기본 CRUD 기능도 스프링 데이터 JPA 가 모두 제공**한다.

  

- **스프링 부트 + JPA + 스프링 데이터 JPA(프레임워크)**

  : 개발 코드들을 줄이고, 핵심 비즈니스 로직 개발에 집중할 수 있다.

    관계형 데이터베이스를 사용한다면 필수로 사용된다.

  

- **스프링 데이터 JPA 구현**

  - 환경 설정 (기존  JPA 설정과 같다)

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

  - 실제 구현

    - repository 경로에 `SpringDataJpaMemberRepository.java` 인터페이스 생성

      ~~~java
       ``` SpringDataJpaMemberRepository
      package hello.hellospring.repository;
      
      import hello.hellospring.domain.Member;
      import org.springframework.data.jpa.repository.JpaRepository;
      
      import java.util.Optional;
      
      public interface SpringDataJpaMemberRepository extends JpaRepository<Member,Long>,MemberRepository {
      
          
          @Override
          Optional<Member> findByName(String name);
          //select m from Member m where m.name = ? 이런식으로 JPA를 짜준다.
          //(이름을 보고 스프링 데이터 JPA가 풀어낸다.)
      
      }
      ```
      ~~~
    
      - interface가 interface를 받을때는 extends 라고한다.
    
      - interface는 다중 상속이 가능하다.
    
      - **스프링 데이터 JPA가 interface에 대한 구현체를  직접 생성, 등록해준다.**
    
      - JPaRepository 안에 기본 CRUD가 구현되어 있다.
    
        
    
    - `SpringConfig.java` 파일에서 스프링 데이터 JPA가 만든 구현체 가져오기
    
      ~~~java
      ```SpringConfig
      @Configuration
      public class SpringConfig {
      //    private DataSource dataSource;
              private final MemberRepository memberRepository;
      
          @Autowired
          public SpringConfig(MemberRepository memberRepository){
              this.memberRepository=memberRepository;
          }
      
          @Bean
          public MemberService memberService(){
              return  new MemberService(memberRepository);
          }
      }
      ```
      ~~~
    
      스프링 데이터 JPA가 interface의 구현체를 직접 만들고, 스프링 빈에 등록을 해놓는다.
    
      그래서 기존의 dataSource를 사용하지 않고, 바로 memberRepository의 형태로 가져올 수 있게 된다.
      
      
      
      ![image-20210814024407975](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210814024407975.png)
