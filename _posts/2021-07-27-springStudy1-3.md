---
title:  "[Spring Study]  -간단한 웹 어플리케이션 개발(3)"
excerpt: "View 환경설정"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-07-27
last_modified_at: 2021-07-27
---
1. ## View 환경설정
- ### resources -> static 경로에 index.html 파일 생성
 
    **(웰컴 페이지 경로 : `static/index.html`)**

   아래 코드 작성

    ```
    <!DOCTYPE HTML>
    <html>
    <head>
        <title>Hello</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    </head>
    <body>
    Hello
    <a href="/hello">hello</a>
    </body>
    </html>
    ```

    다시 HelloSpringApplication을 run 하면, 로컬에서 실행화면이 바뀜

    ![image-20210727014755694](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727014755694.png)

- ### thymeleaf 템플릿 엔진 사용방법

   : 템플릿 엔진 - HTML을 원하는 모양대로 바꿀 수 있음.

   - main-> java-> hello.hellospring->controller 패키지 생성 -> HelloController 클래스 생성

   - resources->templates->hello.html 생성

     ![image-20210727020528837](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727020528837.png)

   ```
   package hello.hellospring.controller;
   import org.springframework.ui.Model;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.GetMapping;
   
   @Controller//MVC 모델 중 컨트롤러. 컨트롤러라는 표시를 해줘야함
   public class HelloController {
       @GetMapping("hello")//hello라는 url에 자동적으로 매핑됨
       public String hello(Model model){
           model.addAttribute("data","hello!");
           return "hello";
       }
   }
   ```

   ```
   <!DOCTYPE HTML>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
     <title>Hello</title>
     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
   </head>
   <body>
   Hello
   <a href="/hello">hello</a>
   <p th:text="'안녕하세요. '+${data}">안녕하세요. 손님</p>
   </body>
   </html>
   ```

   localhost:8080/hello의 경로로 다시 실행하면 data의 value인 "hello!!"가 뒤에 추가된것을 확인할 수 있다.

   ![image-20210727020940722](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727020940722.png)

-> 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(`viewResolver`)가 화면을 찾아서 처리한다.

* 스프링 부트 템플릿엔진 기본 viewName 매핑 

  : `resources:templates/` +{viewName}+`.html` 

- 번외)) 서버 재시작 없이 VIew 파일 변경하는 법
  `spring-boot-devtools` 라이브러리를 추가하면, `html` 파일을 컴파일만 해주면 서버 재시작없이 View 파일 변경 가능

