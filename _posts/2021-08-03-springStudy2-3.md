---
title:  "[Spring Study]  - 회원관리 예제(3) "
excerpt: "회원관리 예제 - 웹 MVC 개발"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-03
last_modified_at: 2021-08-03

---

회원관리 예제 - 웹 MVC 개발

- 회원 웹 기능 -홈 화면 추가

  ~~~java
  ```HomeController
  package hello.hellospring.controller;
  
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;
  
  @Controller
  public class HomeController {
  
      @GetMapping("/")
      public String home(){
          return "home";
      }
  }
  ~~~

  ~~~html
  ```resources/templates/home.html
  <!DOCTYPE HTML>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
  <div class="container">
    <div>
      <h1>Hello Spring</h1>
      <p>회원 기능</p>
      <p>
        <a href="/member/new">회원가입</a>
        <a href="member">회원 목록</a>
      </p>
    </div>
  </div>
  </html>
  ~~~

  참고사항) 실행 우선순위

  ![image-20210807031648506](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210807031648506.png)

  요청을 받으면 스프링 컨테이너에서 해당 컨트롤러가 있는지 먼저 확인

  없으면 static을 실행.

  -> `home.html`이 `index.html` 보다 우선 순위로 실행되는 이유

- 회원 웹 기능 - 등록

  회원가입을 눌렀을 때                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  이동

  ~~~java
  ```MemberController
      @GetMapping("/members/new")
      public String createForm(){
          return "members/createMemberForm";
      }
  ~~~

  ~~~html
  ```templates/members/createMemberForm
  <!DOCTYPE HTML>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
      <title>Hello</title>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
  <div class="container">
      <form action="/members/new" method="post">
          <div class="form-group">
              <label for="name">이름</label>
              <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
          </div>
          <button type="submit">등록</button>
      </form>
  </div>
  </body>
  </html>
  ~~~

  입력 폼 작성 후, 결과 전송                                                                        

  ~~~JAVA
  ```MemberController   
  @PostMapping("/members/new")
          public String create(MemberForm form){
              Member member=new Member();
              member.setName(form.getName());
  
              memberService.join(member);
  
              return "redirect:/"; //홈 화면으로 결과 전송
          }
  ~~~

- 회원 웹 기능 - 조회

  ~~~java
  ```MemberController
      @GetMapping("/members")
      public String list(Model model){
          List<Member> members =memberService.findMembers();
          model.addAttribute("members",members);
          // members라는 객체클래스 생성해서 모든 member 넣기
          return "members/memberList";
      }
  ~~~

  
