---
title:  "[Spring Study]  -간단한 웹 어플리케이션 개발(1)"
excerpt: "사전 준비"

categories: Spring
tags:
  - [Spring, SpringBoot]
toc: true
toc_sticky: true
 
date: 2021-07-27
last_modified_at: 2021-07-27
---

1. ## 사전 준비
- ### 사전 준비
   - Java 11설치

   - IDE:IntelliJ 또는 Eclipse 설치

   - https://start.spring.io <- spring boot 만들어주는 사이트

     ![image-20210727005538643](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727005538643.png)

     * project : 최근에는 Gradle Project 많이 사용 추세
     * Spring Boot : snapshot,m1 정식 릴리즈 아님. 정식 릴리즈 중 가장 최근 것 선택
     * add dependencies : 어떤걸 땡겨서 쓸건지(필요한 것)

- ### 지정 경로에 알집을 풀고 IntelliJ IDEA 에서 열어준다

   ![image-20210727010008101](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727010008101.png)

   * src -> main : java(실제 패키지와 소스파일 있음),resources

   * src-> test : test코드들과 관련된 소스 파일 있음

     => 최근 개발의 트렌드에서는 test가 중요하기 때문에 나눠놓음

     * **build.gradle**

     : 버전 설정, 라이브러리 가져옴

     ![image-20210727010329255](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727010329255.png)

   * sourceCompatibility : 자바 버전

   * dependencies : 포함된 라이브러리들(testImplemetation은 default)

     * .gitignore : 깃에 올라가는 것을 방지하는 환경변수

- ### 일단 시작


   ![image-20210727011317819](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727011317819.png)

    8080 포트에서 실행되는 것을 확인할 수 있다.

    ![image-20210727011403093](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727011403093.png)

    웹 브라우저에서 해당 포트가 로컬에서 띄워졌음을 확인할 수 있다.

    이처럼 스프링 부트를 사용하면 톰캣이 자동으로 함께 실행이 된다. (**스프링부트의 장점**)



번외)) 빌드속도 빠르게 하기

    ![image-20210727011823499](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727011823499.png)

    설정에서 gradle을 검색하고 빌드 설정을 IntelliJ로 변경한다.

    -> gradle을 통하지 않고 바로 자바를 통해서 빌드 되므로 속도가 빨라진다.
