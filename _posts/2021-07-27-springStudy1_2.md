---
title:  "[Spring Study]  -간단한 웹 어플리케이션 개발(2)"
excerpt: "라이브러리 살펴보기"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-07-27
last_modified_at: 2021-07-27
---

1. External Libaries  폴더 확인

![image-20210727012246022](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210727012246022.png)

내가 설정하지 않은 라이브러리들이 많이 포함되어있다. 

->  gradle의 툴들은 "의존관계 관리"를 해준다. 

각 라이브러리가 자신이 필요한 의존관계의 라이브러리들을 자동적으로 딸려서 가져오게 된다.

(spring core까지 가져오게되어 개발자가 아주 편리해졌다.)

![image-20210727012634651](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210727012634651.png)

오른쪽 위에 "gradle"을 눌러보면, 의존관계를 확인할 수 있다.

-> 과거의 방식처럼 톰캣과 자바IDE를 개발자가 연동시킬 필요없이, 코드에서 임베디드 형식으로 톰캣을 가지고 있다. 

기타) 

* junit : 테스트 프레임워크
