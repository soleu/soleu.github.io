---
title:  "[Spring Basic]  - 스프링 핵심 원리 이해(4)"
excerpt: "예제 만들기- 비용 정책 변경"

categories:
  - SpringBasic
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-16
last_modified_at: 2021-08-16
---

## 3. 스프링 핵심 원리 이해 2 - 객체 지향 원리 적용

- ### 새로운 할인 정책 개발

- #### 할인 정책 변경 내용

  - 고정 금액 할인 -> 정률 금액(%) 할인(VIP에게만 10% 할인율 적용)

-> 기존의 `discount`를 인터페이스로 만들어 놨기 때문에, 관련 클래스만 바꿔끼워주면 된다.



- #### 할인 정책 변경 적용 코드

  ![image-20210821214609863](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210821214609863.png)

  `RateDiscountPolicy`클래스를 생성한다.

  - RateDiscountPolicy

    ```java
    package hello.core.discount;
    
    import hello.core.member.Grade;
    import hello.core.member.Member;
    
    public class RateDiscountPolicy implements DiscountPolicy{
        private int discountPercent=10;
    
        @Override
        public int discount(Member member, int price) {
            if(member.getGrade()== Grade.VIP){
                return price * discountPercent/100;
            }else{
                return 0;
            }
        }
    }
    
    ```

    코드가 잘 짜여졌는지 확인할 때는 윈도우 기준 `Ctrl + Shift + T`를 눌러 쉽게 테스트케이스를 생성할 수 있다. 

    관련 내용은 테스트 관련 글 참조 

    

- #### 할인 정책 코드 적용

  현재 코드에서 `RateDiscountPolicy`를 적용하려면 

  `OrderServiceImpl`에서 

  ```java
    private final DiscountPolicy discountPolicy=new FixDiscountPolicy();
  ```

  위의 코드를 아래와 같이 바꿔야한다.

  ```java
    private final DiscountPolicy discountPolicy=new RateDiscountPolicy();
  ```

  여기서 문제점을 발견 할 수 있는데,

  

  지금 코드는 **기능을 확장해서 변경하면 클라이언트 코드에 영향**을 주게 된다.

  따라서 **OCP를 위배**한다.

  ![image-20210821222215494](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210821222215494.png)

  현재 구현한 코드를 그림으로 나타내면 다음과 같다.

  인터페이스에만 의존하는게 아닌, 구현체에도 의존을 하고 있는 것이다.(**DIP 위배**)

  

- 다음장에서 인터페이스에만 의존하는 방향으로 구현하는 방법을 이야기해보자.

참고 : 김영한 - '스프링 핵심 원리 - 기본편'