---
title:  "[Spring Study]  -회원 관리 예제(1)"
excerpt: "백엔드 개발 설계"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-03
last_modified_at: 2021-08-03
---


**Spring Study**  - 회원 관리 예제(1) - 백엔드 개발 설계

- **비즈니스 요구사항**

  - 데이터 : 회원 ID, 이름

  - 기능 : 회원 등록, 조회

  - 아직  데이터 저장소가 선정되지 않음(가상의 시나리오)

    

*일반적인 웹 애플리케이션 계층 구조*

![image-20210803022419713](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210803022419713.png)

*클래스 의존관계*

![image-20210803022607051](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210803022607051.png)

- 아직 데이터 저장소가 선정되지 않아서, 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계
- 데이터 저장소는 RDB,NoSQL 등등 다양한 저장소를 고민중인 상황으로 가정
- 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용
