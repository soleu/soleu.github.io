---
title:  "[Spring Study]  - 테스트케이스"
excerpt: "통합테스트 구현"

categories: Spring
tags:
  - [Spring, SpringBoot,TestCase]
layout : post
toc: true
toc_sticky: true
 
date: 2021-08-14
last_modified_at: 2021-08-14
---

통합 테스트

- 배경
  - 순수한 자바 코드만으로는 DB 등에 연관된 기능들을 테스트 하기가 힘들다.
  - 테스트를 스프링으로 엮어서 표현하는 방법을 구상한다.

- 통합 테스트 구현하기

  - test/java/service 경로에 `MemverServiceIntegrationTest .java` 파일 생성

  - 기존 테스트 코드에서 통합 테스트 용도로 작성

    ```java
    package hello.hellospring.service;
    
    import hello.hellospring.domain.Member;
    import hello.hellospring.repository.MemberRepository;
    import hello.hellospring.repository.MemoryMemberRepository;
    import org.junit.jupiter.api.AfterEach;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;
    
    import javax.transaction.Transactional;
    
    import static org.assertj.core.api.Assertions.assertThat;
    import static org.junit.jupiter.api.Assertions.assertThrows;
    
    @SpringBootTest
    @Transactional
    class MemberServiceIntegrationTest {
    
    
    //    @BeforeEach
    //    public void beforeEach(){
    //        memberRepository = new MemoryMemberRepository();
    //        memberService = new MemberService(memberRepository);
    //    }
        //직접 객체 생성 하지 않고
    
        @Autowired MemberService memberService;
        @Autowired MemberRepository memberRepository; //MemoryMemberRepository 아님
    
    //    @AfterEach
    //    public void afterEach(){memberRepository.clearStore();}
    
        @Test
        void 회원가입() {
            //given : 뭔가가 주어졌는데
            Member member = new Member();
            member.setName("spring");
            //when : 이걸 실행했을 때
            Long saveId =memberService.join(member);
    
            //then : 이런 결과가 나와야한다.
            Member findMember=memberService.findOne(saveId).get();
            assertThat(member.getName()).isEqualTo(findMember.getName());
        }
        @Test
        public void 중복_회원_예외(){
            //given
            Member member1=new Member();
            member1.setName("spring");
    
            Member member2=new Member();
            member2.setName("spring");
    
            //when
            memberService.join(member1);
            //콤마 뒤를 실행할건데, 에러가 나면 에러코드를 출력해라
            IllegalStateException e = assertThrows(IllegalStateException.class,()-> memberService.join(member2));
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    
        }
    
    }
    
    ```

    `BeforeEach` : 

    이전에는 테스트를 할때 다른 모듈과 같은 변수를 초기화를 시키기 위해 사용했다.

    해당 경우에서도 테스트를 할 경우, DB에 insert query 등 DB 내의 정보를 변경하는 경우(오류)가 발생하게 된다. 

    `**Transactional**` : 

    기존에 DB에 값이 변경되기 위해서는  `commit`을 해줘야 반영이 되는데(설정을 해주지 않을 경우 `auto commit`로 실행 됨)

    `@Transactional`을 사용하게 되면 `commit`을 하기전(테스트가 끝날때) **`rollback`을 시킴으로써 테스트의 값 변경이 DB에 반영되지 않도록 한다.** 

     즉, 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜젝션을 시작하고, 테스트 완료 후에 항상 `rollback`한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.

    (**해당 기능은 Test에서만 동작되는 기능이다.**)

    `@springBootTest` : 

    스프링 컨테이너와 테스트를 함께 실행한다.



- 추가) 

  - 단위 테스트가 좋은 테스트일 경우가 대개 많다. 따라서 스프링 컨테이너 없이 테스트 하는 것을 지향해야한다. 
  
  - `@Commit`을 통해 메서드나 클래스가 끝내고 `commit`을 실행하게 할 수 있다.
  
  - `wrong user name or password` 오류 
  
  스프링 부트 2.4부터 `application.properties` 에 `spring.datasource.username=sa`를 추가해줘야 함

