---
title:  "[Spring Study]  - AOP"
excerpt: "AOP(Aspect Oriented Programming)"

categories: Spring
tags:
  - [Spring, SpringBoot,TestCase]
layout : post
toc: true
toc_sticky: true
 
date: 2021-08-14
last_modified_at: 2021-08-14
---

- AOP

  - AOP가 필요한 상황

    - 모든 메소드의 호출 시간을 측정하고 싶다면?

      -> 기존에는 모든 메서드 시작과 끝에 시간을 재는 코드를 넣어야 함

      -> 비효율적이고, 복잡함

    - 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)

      - 핵심 관심 사항 : 핵심 로직. 해당 메서드에서 구현하고자 하는 중심 로직

      - 공통 관심 사항 : 공통 로직. 여러 메서드에 들어가는 공통적인 로직 

      -> 공통 로직(공통 관심 사항)이 비즈니스 로직에 섞여서 유지보수가 불편함

    - 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면? 

    

  - AOP 적용
  
    - AOP : Aspect Oriented Programming
    
    - 공통 관심 사항과 핵심 관심 사항을 분리시킨다.
    
      ![image-20210814030548607](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210814030548607.png)
    
    
    
  - AOP 구현
  
    - src/main/java/hello.hellospring 경로에  aop 패키지 생성
  
      ````java
      ```
      @Aspect 
      @Component
      public class TimeTraceAop {
          @Around("execution(* hello.hellospring..*(..))")
          //적용할 패키지경로
          public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
              long start= System.currentTimeMillis();
              System.out.println("START"+joinPoint.toString());
      
              try{
                  return joinPoint.proceed();
              }finally {
                  long finish = System.currentTimeMillis();
                  long timeMs=finish-start;
                  System.out.println("END: "+joinPoint.toString()+" "+timeMs+"ms");
      
              }
          }
      }
      ```
      ````
      
      `@Aspect` : AOP 애노테이션
      
      `@Component` :bean에 등록
      
      `@Around("execution(* hello.hellospring..*(..))")` : 적용할 패키지경로
      
      ~~~java
          ```SpringConfig
          @Bean //SpringBean에 등록
          public TimeTraceAop(){
              return new TimeTraceAop();
          }
          ```
      
      ~~~
      
      bean에 등록하는 방법은 위와 같이 Spring Config를 통해 직접 등록할 수도 있다.
      
      
    
  - AOP 사용 장점
  
    - 핵심사항과 공통 관심 사항 분리 가능
    - 핵심 관심 사항 깔끔하게 유지 가능
    - 변경이 필요할 시, 이 로직만 변경하면 되기 때문에 유지 보수 용이
    - 원하는 적용 대상 선택 가능(``@Around`)
  
  - AOP 작동 원리
  
    ![image-20210814032506257](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210814032506257.png)
  
    기존의 스프링 컨테이너는 Controller에서 Service를 호출하는 방식으로 사용되었다.
  
    ![image-20210814032543887](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210814032543887.png)
  
    AOP를 사용하게 되면 가짜스프링빈(프록시)를 먼저 호출하게 하고, 프록시의 동작이 끝났을때 `joinPoint.proceed`라는 코드를 통해 실제 Service가 동작할 수 있도록 한다.
  
    ![image-20210814032815096](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210814032815096.png)

