---
title:  "[Spring Basic]  - 스프링 핵심 원리 이해(5)"
excerpt: "의존관계 주입(DI)"

categories:
  - SpringBasic
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-17
last_modified_at: 2021-08-17
---

## 3. 스프링 핵심 원리 이해 2 - 의존관계 주입(DI)

- 인터페이스에만 의존하도록 설계 변경하기

  ```java
  public class OrderServiceImpl implements OrderService {
  //private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
  private DiscountPolicy discountPolicy;
  }
  ```

  위와같이 코드를 짜면 인터페이스에만 의존하도록 할 수 있다.

  그리고, 구현체를 연결하는 부분을`AppConfig`라는 클래스를 생성하여 구현한다.

  

- AppConfig 

  :애플리케이션의 전체 동작 방식을 구성(config)하기 위해, **구현 객체를 생성**하고, **연결**하는 책임을 가지는 별도의 설정 클래스

  `hello.core`패키지 바로 아래 `AppConfig.java`를 생성한다.

  ```java
  public class AppConfig {
      public MemberService memberService(){
          return new MemberServiceImpl(new MemoryMemberRepository());//여기서 생성자 주입
      }
  
      public OrderService orderService(){
          return new OrderServiceImpl(new MemoryMemberRepository(),new FixDiscountPolicy());
      }
  }
  ```

  이와 같이 구현체를 연결한다.

   

  - 번외) 코드 중복 제거

    ```java
    public class AppConfig {
        
         private MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
        
        public MemberService memberService(){
           // return new MemberServiceImpl(new MemoryMemberRepository());
        }
    
        public OrderService orderService(){
           // return new OrderServiceImpl(new MemoryMemberRepository(),new FixDiscountPolicy());
            
        }
        
        public MemberService memberService(){
    
            return new MemberServiceImpl(memberRepository());
        }
        
        public DiscountPolicy discountPolicy(){
            return new FixDiscountPolicy();
        }
    
        public OrderService orderService(){
            return new OrderServiceImpl(memberRepository(),discountPolicy());
        }
        
    }
    ```

    `MemeberService` 과  `OrderService`에서  `MemorMemberRepository` 라는 같은 return 형식을 사용한다. 

     `MemberRepository memberRepository`의 선언으로 하나로 묶으면서 코드 중복을 피할 수 있다. 

    

- 서비스 코드 변경

  ~~~java
  ```MemberServiceImpl    
  	private final MemberRepository memberRepository;
  
      public MemberServiceImpl(MemberRepository memberRepository){
          this.memberRepository=memberRepository;
      }//생성자를 통해 뭐가 들어갈지 넣어 줄 것임
  ```
  ~~~

  위와 같이 구현체를  변경한다. 생성자를 통해 appConfig에서 선택한 구현체를 추상체(인터페이스)만을 사용하여 넣어준다.

  

  - `MemberRepository`와 같은 추상체는 인터페이스만 의존한다.

  - `MemberServiceImpl`와 같은 구현체는 생성자를 통해 어떤 객체를 주입할지는 오직 외부 `AppConfig` 에서 결정된다.

  - 이렇게 하면 **구현체**는 **의존 관계에 대한 고민은 외부**에 맡기고, 실행해만 집중 할 수 있다.
  - 객체의 생성과 연결은 `AppConfig`가 담당
  - DIP 완성 : `MemberServiceImpl` 은 `MemberRepository` 인 추상에만 의존

  

- 테스트 코드 변경

  ~~~java
  ``` MemberServiceTest
  public class MemberServiceTest {
      MemberService memberService; //위에서 추상체로만 선언
      @BeforeEach //메서드가 실행되기전 의존관계 주입(구현체 연결)
      public void beforeEach(){
          AppConfig appConfig=new AppConfig();
          memberService = appConfig.memberService();
      }
      
        @Test
      void join(){ //각 메서드에서는 기존과 같은 코드로 사용할 수 있다.
          //given
          Member member =new Member(1L,"memberA",Grade.VIP);
          //when
          memberService.join(member);
          Member findMember =memberService.findMember(1L);
  
          //then
          Assertions.assertThat(member).isEqualTo(findMember);
      }
  }
  ```
  ~~~

  이와 같이 테스트 코드에서는 메서드가 실행되기전 `@BeforeEach`로 의존 관계를 주입하여 사용한다.

  

![image-20210822000211334](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210822000211334.png)

`AppConfig` 를 사용하면 사용영역과 구성 영역이 분리된다. 

사용하는 구현체가 변경된다하더라도, 사용영역은 영향을 받지 않는다. 


참고 : 김영한 - '스프링 핵심 원리 - 기본편'
