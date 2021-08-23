---
title:  "[Spring Basic]  - 스프링 핵심 원리 이해(1)"
excerpt: "예제 만들기 - 프로젝트 생성"

categories:
  - SpringBasic
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-13
last_modified_at: 2021-08-13
---

2. ## 스프링 핵심 원리 이해 1 - 예제 만들기

- 기본 프로젝트 생성은 `spring 기초`에서와 같다.

  - https://start.spring.io

  - 프로젝트 선택

    - Project : Gradle Project
    - Spring Boot : 2.3.x (가장 안정화된 최신버전. M1(x),snapshpt(x))
    - Language : Java
    - Packaging : Jar
    - Java : 11

  - Project Metadata

    - groupId : hello
    - artifactId : core

  - Dependencies :  선택하지 않는다.

    ![image-20210819023649549](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210819023649549.png)

  

-> generate를 해서 알집 파일을 만들어 준 후,

intellij에서 **해당 경로에 build.gradle를 import해오는 방식**으로 열어야 자동으로 빌드가 설정된다. 



추가) 설정에서 빌드 속도 높이는 법

: intellij에서 java를 바로 실행하므로 속도가 더 빨라진다.

![image-20210819025754497](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210819025754497.png)


참고 : 김영한 - '스프링 핵심 원리 - 기본편'
