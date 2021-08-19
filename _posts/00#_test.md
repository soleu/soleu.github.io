2. ## 스프링 핵심 원리 이해 1 - 예제 만들기

- ### 테스트 하기

  - 순수 자바로 테스트하기

  

  ![image-20210820020513534](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820020513534.png)

  ​		위와 같은 경로에 `MemberApp`이라는 테스트용 클래스 생성

    -   - MemberApp

        ```java
        package hello.core;
        
        import hello.core.member.Member;
        import hello.core.member.Grade;
        import hello.core.member.MemberService;
        import hello.core.member.MemberServiceImpl;
        
        public class MemberApp { //순수 자바로 테스트 하는 방법
            public static void main(String[] args) {
                MemberService memberService=new MemberServiceImpl();
                Member member = new Member(1L,"memberA",Grade.VIP);
                memberService.join(member);
        
                Member findMember = memberService.findMember((1L));
                System.out.println("findMember = " + findMember.getName());
                System.out.println("new member"+member.getName());
        
            }
        }
        
        ```

  - **JUnit 을 사용하여 Test 하기**

    ![image-20210820020803545](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820020803545.png)

    `test`에 위와 같은 패키지와 파일 생성

    

    - MemberServiceTest

  ```java
  package hello.core.member;
  
  import org.assertj.core.api.Assertions;
  import org.junit.jupiter.api.Test;
  
  public class MemberServiceTest {
      MemberService memberService =new MemberServiceImpl();
  
      @Test
      void join(){
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

  이와 같은 방법으로 하는 것이 테스트 케이스 관리에 용이하고, 유지보수가 쉽다.

  따라서 필수적으로 사용할 줄 알아야한다.

  

  

