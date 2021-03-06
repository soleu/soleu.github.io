---
title:  "[Spring Basic]  - 스프링 핵심 원리 이해(2)"
excerpt: "예제 만들기- 회원 도메인 설계"

categories:
  - SpringBasic
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-14
last_modified_at: 2021-08-14
---

2. ## 스프링 핵심 원리 이해 1 - 예제 만들기

- ### 비즈니스 요구사항과 설계

   ![image-20210819030315114](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210819030315114.png)



- #### 회원 도메인 설계

- 회원 도메인 요구사항 정리

  - 회원을 가입하고 조회할 수 있다.
  - 회원은 일반과 VIP 두 가지 등급이 있다.
  - 회원 데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동할 수 있다.(미정)



![image-20210819030621299](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210819030621299.png)

**역할과 구현을 구분해서 저장소를 interface로 만들어 바꿀 수 있도록 만든다.**

- 회원 서비스 : `MemberServiceImpl`
- 클래스 다이어그램 : interface를 포함한 모든 클래스의 연관관계를 정의(정적)

- 객체 다이어그램 : 실제로 (new 로 만드는) 객체만을 표기해 놓은 것(동적)

  

- ### 회원 도메인 개발

  ![image-20210820013233435](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820013233435.png)

해당 경로에 member 패키지를 생성하고, 위와 같이 레포지토리와 서비스를 각각 인터페이스와 클래스로 설정해줄 것이다.

- Grade (enum : 단순 열거형)

  ```java
  package hello.core.member;
  
  public enum Grade {
      BASIC,
      VIP
  }
  ```

- Member

  ```java
  package hello.core.member;
  
  public class Member {
      private Long id;
      private String name;
      private Grade grade;
  
      public Member(Long id,String name,Grade grade){
          this.id=id;
          this.name=name;
          this.grade=grade;
      }
  
      public Long getId() {
          return id;
      }
  
      public String getName() {
          return name;
      }
  
      public Grade getGrade() {
          return grade;
      }
  
      public void setId(Long id) {
          this.id = id;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public void setGrade(Grade grade) {
          this.grade = grade;
      }
  }
  ```

  윈도우 기준 Alt+Insert 키로 constructor,getter/setter를 쉽게 작성할 수 있다.

- MemberRepository (interface)

  ```java
  package hello.core.member;
  
  public interface MemberRepository {
      void save(Member member);
  
      Member findById(Long memberId);
  
  }
  ```

- MemoryMemberRepository(class)

  ```java
  package hello.core.member;
  
  import java.util.HashMap;
  import java.util.Map;
  
  public class MemoryMemberRepository implements MemberRepository {
  
      private Map<Long,Member> store =new HashMap<>();
  
      @Override
      public void save(Member member) {
      store.put(member.getId(),member);
      }
  
      @Override
      public Member findById(Long memberId) {
          return store.get(memberId);
      }
  }
  ```

​     

- MemberSerivce(interface)

  ```java
  package hello.core.member;
  
  public interface MemberService {
  
      void join(Member member);//가입
      Member findMember(Long memberId); //회원찾기
  }
  ```

- MemberServiceImpl(class)

  ```java
  package hello.core.member;
  
  public class MemberServiceImpl implements  MemberService{
  
      private final MemberRepository memberRepository=new MemoryMemberRepository();
  
      @Override
      public void join(Member member) {
          memberRepository.save(member);
      }
  
      @Override
      public Member findMember(Long memberId) {
          return memberRepository.findById(memberId);
      }
  }
  
  ```

  

이렇게 하면 회원 도메인 개발이 완성되었다. 이제 주문과 할인 도메인을 개발하여야한다.

참고 : 김영한 - '스프링 핵심 원리 - 기본편'