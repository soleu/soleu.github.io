---
title:  "[Spring Study]  - 스프링 특징(1) "
excerpt: "의존성 주입 및 자바 빈 "

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-06
last_modified_at: 2021-08-06

---

의존성 주입 및 자바 빈

- **DI(Dependency Injection : 의존성 주입)**

  : 외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴으로, 인터페이스를 사이에 둬서 클래스 레벨에서는 의존관계가 고정되지 않도록 하고 런타임 시에 관계를 다이나믹하게 주입하여 유연성을 확보하고 결합도를 낮출 수 있게 해준다.

  *주의할 점 :  한 객체를 다른 객체에 주입하기 위해서는 애플리케이션 실행 시점에 필요한 객체(빈)을 생성하여야 하며, 의존성이 있는 두 객체를 연결하기 위해 한 객체를 다른 객체로 주입 시켜야 한다.

  (다른 빈을 주입받으려면, 자기 자신이 반드시 컨테이너의 빈이어야 한다.)

  - 두 객체 간의 관계라는 관심사의 분리

  - 두 객체 간의 결합도를 낮춤

  - 객체의 유연성을 높임

  - 테스트 작성을 용이하게 함

  

  MemberService에서 

  ```
  MemoryMemberRepository memberRepository =new MemberRepository();
  ```

  로 새 클래스 객체를 만들고, 

  MemberServiceTest에서 다시 같은 상황을 반복하면, 실행할때 각각 새로운 리포지토리 객체를 가져오게 된다. 

  이를 방지하기 위해 사용하는 코드는

  -MemberSerivce 

  ```java
      //memberRepository를 생성과 동시에 내부에서 선언해줄 수 있도록 함
      private  final MemberRepository memberRepository;
  
      public MemberService(MemberRepository memberRepository){
          this.memberRepository=memberRepository;
      }
  ```

  -MemberSerivce Test

  ```java
      MemoryMemberRepository memberRepository;
      MemberService memberService;
  
      @BeforeEach
  //각 클래스를 실행하기 전에 각각의 멤버 서비스에 리포지토리를 넣어줌.(동일한 리포지토리 사용)
      //DI(Dependency Injection : 의존성 주입)
      public void beforeEach(){
          memberRepository=new MemoryMemberRepository();
          memberService = new MemberService(memberRepository);
      }
  ```

  이런식으로 같은 클래스 객체를 사용하도록 한다.



- **스프링 빈과 의존관계**

  MVC 패턴으로 만들려면, View, Controller, Model 가 필요하다.

  이 중에서 Controller를 먼저 만들어볼 것이다.

  -> Member Controller가 Member Service 를 통해 회원가입, 중복인증 등의 기능을 수행하도록 설계할 것이다.

  -> **Member Controller 가 Member Service를 의존한다.**

  ![image-20210807000851293](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210807000851293.png)

스프링 컨테이너에서 스프링 빈이 관리된다.

초록색이 bean.

helloController에 @controller 이런식으로 정의를 해주면 bean이 알아서 관리를 한다.(알아서 동작을 하게된다.)

![image-20210807002942082](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210807002942082.png)

- **스프링 빈을 등록하는 2가지 방법**

  - **컴포넌트 스캔**과 자동 의존관계 설정 (컴포넌트 스캔 : 어노테이션을 하는 방식(@Repository ...))

    - `@Component` 에노테이션이 있으면 스프링 빈으로 자동 등록 된다.

    - `@Controller` 컨트롤러가 스프링 빈으로 자동 등록된 이유도 **컴포넌트 스캔 때문**이다.

    - `@Component` 를 포함하는 **다음 에노테이션도 스프링 빈으로 자동 등록**된다.

      - `@Controller`

      - `@Service`

      - `@Repository`

        

  - **자바 코드**로 직접 스프링 빈 등록하기

    참고) 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, **기본으로 싱글톤**으로 등록한다.

    (유일하게 하나만 등록해서 공유한다.)따라서 **같은 스프링 빈이면 모두 같은 인스턴스다.**

    설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.

    장점 : 코드를 바꾸지않고, 구현 클래스를 쉽게 변경할 수 있다.

    

- 참고 사항

  - 실무에서는 주로 **정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔**을 사용한다.

    그리고 정형화 되지 않거나, **상황에 따라 구현 클래스를 변경해야 하면 설정을 통해** 스프링 빈으로 등록한다.

    

참조 : https://mangkyu.tistory.com/150 [MangKyu's Diary]
