---
title:  "[Spring Study]  -간단한 웹 어플리케이션 개발(4)"
excerpt: "cmd로 서버 켜기"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-07-27
last_modified_at: 2021-07-27
---
1. ## cmd로 서버 켜기
- ### 서버를 멈춘다. (초록색 시작버튼으로 있어야 함)

![image-20210727021808371](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727021808371.png)

- ### **윈도우 기준**

   cmd를 켜고, 해당 폴더가 있는 곳 까지 감

   ![image-20210727022645430](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727022645430.png)

   - gradlew.bat build 실행
   - cd build/libs

    ![image-20210727022805831](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210727022805831.png)

    - `hello-spring-0.0.1-SNAPSHOT.jar ` 부분을 복사해서 명령어 창에 붙여넣기

    -> 이전과 똑같이 실행되는 것을 크롬에서 확인할 수 있다.

- ### 추가) already in use 라고 뜰때

    cmd 에서 netstat -ano 를 입력하고, 해당 포트번호를 사용하고 있는 PID 찾기

    taskkill /f /pid [PID번호]를 통해 프로세스 종료 가능
