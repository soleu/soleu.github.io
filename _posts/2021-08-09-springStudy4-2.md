---
title:  "[Spring Study]  -스프링 DB 기술(2)"
excerpt: "JDBC Template 연결 방식 구현 "

categories: Spring
tags:
  - [Spring, SpringBoot,H2,DB,Database]
layout : post
toc: true
toc_sticky: true
 
date: 2021-08-09
last_modified_at: 2020-08-09


---

스프링 DB 기술 -JDBC Template 연결 방식 구현

- 데이터 베이스 연결 방식 (java <->  jdbc <-> db)

  - 순수 Jdbc : 이전에 했던 수동적인 방식

  - 스프링 JdbcTemplate : JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야한다.

  - **JPA** : **기존 코드의 반복 코드 제거, 기본적인 SQL도 자동으로 처리 가능**

    - JPA를 사용하면, SQL과 데이터 중심의 설계에서 **객체 중심**의 설계로 패러다임 전환 가능
    - 개발 **생산성** 크게 높일 수 있음

    

- JDBC Template 연결 방식 구현

  - 환경 설정

    - build.gradle 파일에 jdbc,h2 데이터베이스 관련 라이브러리 추가

      ~~~java
      ```
      implementation 'org.springframework.boot:spring-boot-starter-jdbc'
      runtimeOnly 'com.h2database:h2'
      ```
      ~~~

    - 스프링 부트 데이터베이스 연결 설정 추가(main/resources/application.properties)

      ~~~java
      ```
      spring.datasource.url=jdbc:h2:tcp://localhost/~/test
      spring.datasource.driver-class-name=org.h2.Driver
      ``` //빨간줄 뜨면 build.gradle에서 갱신해주기(코끼리 버튼)
      
      ~~~

  - DB 연결 

    - repository 경로에 `JdbcTemplateMemberRepository` 생성

    ```java
    package hello.hellospring.repository;
    
    import hello.hellospring.domain.Member;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jdbc.core.JdbcTemplate;
    
    import javax.sql.DataSource;
    import java.util.List;
    import java.util.Optional;
    
    public class JdbcTemplateMemberRepository implements  MemberRepository{
    
        private final JdbcTemplate jdbcTemplate;
    
        @Autowired  //생성자가 1개만 있으면 Autowired 생략 가능
        JdbcTemplateMemberRepository(DataSource dataSource){
            jdbcTemplate=new JdbcTemplate(dataSource);
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

    MemberRepository를 implement 해주고, DataSource를 injection 받는다.

    

    - **jdbcTemplate를 이용하여 DB를 연결한다.** (위 코드에서 수정)

      - findById

      ```java
          @Override
          public Optional<Member> findById(Long id) {
              //query문 : ("query문",결과값,쿼리 값) 
              List<Member> result =jdbcTemplate.query("select * from member where id= ? ",memberRowMapper(),id);
              return result.stream().findAny(); //List -> Optional
          }
      
          private RowMapper<Member> memberRowMapper(){
              return (rs,rowNum) -> {
                  Member member=new Member();
                  member.setId(rs.getLong("id"));
                  member.setName(rs.getString("name"));
                  return member;
              };
          }
      ```

      결과 값을 rowMapper로 매핑하여 출력하도록 한다.

      - save

        ```java
            public Member save(Member member) {
                //simpleJdbcInsert를 사용해 쿼리문을 작성하지 않고 해결
                SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
                jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
        			
                Map<String,Object> parameters = new HashMap<>();
                parameters.put("name",member.getName());
        
                //키를 받아서 만들고, member에 넣어준다.
                Number key= jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
                member.setId(key.longValue());
                return member;
            }
        ```

    - findByName

      ```java
          @Override
          public Optional<Member> findByName(String name) {
              List<Member> result =jdbcTemplate.query("select * from member where name= ? ",memberRowMapper(),name);
              return result.stream().findAny(); //List -> Optional
          }
      ```

      findById와 동일하다.

      

    - findAll

      ```java
          @Override
          public List<Member> findAll() {
              return jdbcTemplate.query("select * from memer",memberRowMapper());
          }
      ```

  - JDBC - DB 연동 (main/SpringConfig)

    ```java
    package hello.hellospring;
    
    import hello.hellospring.repository.JdbcTemplateMemberRepository;
    import hello.hellospring.repository.MemberRepository;
    import hello.hellospring.repository.MemoryMemberRepository;
    import hello.hellospring.service.MemberService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    import javax.sql.DataSource;
    
    @Configuration
    public class SpringConfig {
        private DataSource dataSource;
    
        @Autowired
        public SpringConfig(DataSource dataSource){
            this.dataSource=dataSource;
        }
    
        @Bean
        public MemberService memberService(){
            return  new MemberService(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository(){
            //return new MemoryMemberRepository();
            return new JdbcTemplateMemberRepository(dataSource);
        }
    }
    
    ```

    

