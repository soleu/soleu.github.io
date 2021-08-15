---
title:  "[Spring Study]  -간단한 웹 어플리케이션 개발(5)"
excerpt: "웹 어플리케이션의 종류와 기초"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-03
last_modified_at: 2021-08-03
---

**Spring Study**  -간단한 웹 어플리케이션 개발(5) -스프링 웹 개발 기초

**[웹 어플리케이션의 종류]**

- 정적 컨텐츠

  : 파일을 **그대로** 웹에 내려주는 것

  

- MVC와 템플릿 엔진

  템플릿 엔진 - **서버에서 동적**으로 프로그래밍을 해서 HTML로 주는 것

  ex) php, jsp

- **API**

  : 클라이언트에게 json이라는 포맷으로 데이터를 전달.

  서버끼리 통신할때.

  

  **[구현 방법]**

  1. 정적 컨텐츠

     ![image-20210803002006013](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803002006013.png)

     

  static 폴더에 파일 생성.

  ![image-20210803002039281](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803002039281.png)

  해당 경로로 들어가면 연결 됨

![image-20210803002141082](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803002141082.png)

2. MVC 와 템플릿 엔진

   - MVC : Model,View,Controller

     - Controller : 로직적인 부분

       ````
       ```java
       @Controller
       public class HelloController{
       	
           @GetMapping("hello-mvc")
           //requried= default가 true 값. 값이 없을때 오류가 뜰 수 있음.
           public String hellowMvc(@RequestParam(name="name",required = false)String name,Model model){
               model.addAttribute("name",name);
               return "hello-template";
           }
       }
       ```
       ````

       

     - View: 보여주는 부분에 집중해야함

       `resources/template/hello-template.html`

       ```
       ```html
       <html xmlns:th="http://www.thymeleaf.org">
       <body>
       <p th:text="'hello '+${name}">hello! empty</p>
       </body>
       </html>
       
       ```

       

![image-20210803015418611](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803015418611.png)

![image-20210803015435753](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803015435753.png)



변환을 한다라는 점이 정적 컨텐츠와의 차이를 보인다.



3. API

   ```
      ```java
      @Controller
      public class HelloController{
       @GetMapping("hello-api")
       @ResponseBody
       public Hello helloApi(@RequestParam("name")String name) {
           Hello hello = new Hello();
           hello.setName(name);
           return hello;
       }
           static class Hello {
               private String name;
   
               public String getName() {
                   return name;
               }
               public void setName(String name){
                   this.name=name;
               }
           }
       }
   ```

   

![image-20210803020638465](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803020638465.png)



json 형태로 출력



![image-20210803020821152](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210803020821152.png)

기본 정책 : responseBody가 오면 문자라면 String converter가, 다른 것들이면 json으로 바꿔서 전달

`@ResponseBody` 사용

- HTTP의 BODY에 문자 내용을 직접 반환
- `viewResolver` 대신에 `HttpMessageConverter`가 동작
- 기본 문자처리 : `StringHttpMessageConverter`
- 기본 객체처리: `MappingJackson2HttpMessageConverter`
- byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어있음



 참고) ctrl+n : Getter and Setter 빨리 가져올 수 있음
