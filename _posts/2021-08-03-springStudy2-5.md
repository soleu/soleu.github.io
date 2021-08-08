---
title:  "[Spring Study]  - 회원관리 예제(5) "
excerpt: "회원 리포지토리 테스트 케이스 작성법"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-03
last_modified_at: 2020-08-03


---

회원 리포지토리 테스트 케이스 작성법

- 테스트 방법

  : 

  자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능 실행

-> 준비하고 실행하는데 오래걸림, 반복 실행하기 어려움. 여러 테스트를 한번에 실행하기 어려움

=> JUnit을 통해 테스트 실행



- TestCase

  -> test package에서 같은 repository package를 만들어 테스트케이스 작성

  클래스 명 : '테스트할 클래스 이름'+Test

  @Test를 붙여서 작성

  - 각각의 메서드 별로 테스트를 진행할 수 있고, 클래스를 실행하면, 전체 테스트케이스를 한번에 실행 가능

  ```
  @Test //save 메서드 실행
      public void save(){
          Member member =new Member();
          member.setName("spring");
  
          repository.save(member);
  
          Member result=repository.findById(member.getId()).get();//optional 에서 꺼낼때는 .get()
          Assertions.assertEquals(member,result);//두개가 동일한지 확인
          //assertThat(member).isEqualTo(result);//위와 동일한 방법이나, 요즘 쓰는 방식
      }
  ```

  

  주의할 점) 

  - 테스트 케이스 중 어떤 것이 먼저 실행 될지는 알 수 없음.

  - 중복되는 변수가 있다면, 로직 문제가 아닌, 테스트 케이스의 오류로 오류 발생 가능

    -> 테스트 하나가 끝나면 데이터를 clear 시켜줘야 함 

    ```
    @AfterEach //각 메서드가 끝날때마다 동작하도록 만듬
        public void afterEach(){
            repository.clearStore();
        }
    ```

    *** 테스트는 서로 의존관계가 있으면 안된다. ***

    - 개발 -> 테스트 : 일반적
    - 테스트 -> 개발 : Test 주도 개발 (TDD)
