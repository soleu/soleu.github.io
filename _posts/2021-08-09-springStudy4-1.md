---
title:  "[Spring Study]  -스프링 DB 기술(1)"
excerpt: "H2 데이터베이스 설치 "

categories: Spring
tags:
  - [Spring, SpringBoot,H2,DB,Database]
layout : post
toc: true
toc_sticky: true
 
date: 2021-08-09
last_modified_at: 2021-08-09

---

스프링 DB 기술 -데이터베이스 설치

데이터베이스가 없는 상태에서는 서버를 내리면 사용자의 정보가 저장되지 않는다.

데이터 베이스를 설치하고, 서버와 데이터베이스를 연동하는 방법까지 알아보자.



- H2 Database 설치

  : 개발이나 테스트 용도로 가볍고 편리한 DB, 웹 화면 제공

  - https://www.h2database.com

  - h2 데이터베이스 버전은 **스프링 부트 버전**에 맞춘다.

  - 권한 주기 : `chmod 755 h2.sh` (Mac 만)

  - 데이터베이스 파일 생성 방법

    - 설치 후,h2 console 앱 클릭

      ![image-20210809021258685](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809021258685.png)

      ![image-20210809021526157](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809021526157.png)

    - 연결을 누르면, DB 생성

      ![image-20210809021715350](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809021715350.png)

    - `~/test.mv.db` 파일 생성 확인

    - 이후부터는 `jdbc:h2:tcp://localhost/~/test` 이렇게 접속

      ![image-20210809022107755](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809022107755.png)

      : 파일에 직접 접근하는게 아니라 통해서 접근하게 해서 여러 곳에서 접근할 수 있도록 함.

      

- DB 조작하기

  - SQL문을 작성하고 실행하면 결과를 바로 확인할 수 있다. 
  - member 테이블 생성하기

  ![image-20210809022518302](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809022518302.png)

  ```sql
  drop table if exists member CASCADE; //table 생성(이미 있을시엔 작성 X)
  create table member
  (
       id bigint generated by default as identity, //null값일때 db가 기본으로 id값을 채워줌
       name varchar(255),
       primary key (id)
  );
  ```

  해당 코드를 작성하고 실행버튼을 누르면, 왼쪽에서 생성된 table을 확인할 수 있다.

  

  ![image-20210809022948428](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809022948428.png)

  이런 식으로 DB안에 데이터 저장, 조회이 가능하다.



참고 사항) 

![image-20210809023253857](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210809023253857.png)

프로젝트 파일에도 sql 폴더로 따로 관리해주면 파일 내용을 쉽게 관리할 수 있다.

추가) **H2 DB를 사용하면서 DB를 끄게 되면, 프로젝트가 정상 작동이 안될 수 있다.**