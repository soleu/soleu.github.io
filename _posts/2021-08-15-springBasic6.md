---
title:  "[Spring Basic]  - 스프링 핵심 원리 이해(3)"
excerpt: "예제 만들기- 주문과 할인 도메인 설계"

categories:
  - SpringBasic
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-15
last_modified_at: 2021-08-15
---

2. ## 스프링 핵심 원리 이해 1 - 예제 만들기

- ### 주문과 할인 도메인 설계

- 주문과 할인 정책
  - 회원은 상품을 주문할 수 잇다.
  - 회원 등급에 따라 할인 정책을 적용할 수 있다.
  - 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라.(미확정)
  - 할인 정책은 변경 가능성이 높다. 회사의 기본 할인 정책을 아직 정하지 못했고, 오픈 직전까지 미루고 싶다. 최악의 경우 할인을 적용하지 않을 수도 있다.(미확정)

![image-20210820021427919](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820021427919.png)

![image-20210820021609849](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820021609849.png)

![image-20210820021626316](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820021626316.png)

구현체만 바꿈으로 쉽게 재사용할 수 있다.

예) FixDiscountPolicy, RateDiscountPolicy 의 정책 두개를 하나의 인터페이스로 적용함으로 쉽게 바꿔치기(?)가능



- ### 주문과 할인 도메인 개발

- 할인 도메인 개발(discount)

  ![image-20210820022641509](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820022641509.png)

  위와 같이 인터페이스와 클래스를 생성한다.

  - DiscountPolicy(interface)

    ```java
    package hello.core.discount;
    
    import hello.core.member.Member;
    
    public interface DiscountPolicy {
        /**
         * @return 할인 대상 금액
         */
        int discount(Member member,int price);
    }
    ```

  - FixdiscountPolicy(class)

    ```java
    package hello.core.discount;
    
    import hello.core.member.Grade;
    import hello.core.member.Member;
    
    public class FixdiscountPolicy implements DiscountPolicy {
        private int discountFixAmount =1000;//1000원 할인
    
        @Override
        public int discount(Member member, int price) {
            if(member.getGrade()== Grade.VIP){
                return discountFixAmount;
            }else {
                return 0;
            }
        }
    }
    ```

- 주문 도메인 개발(order)

  ![image-20210820023944900](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210820023944900.png)

  위 경로처럼 order 패키지 및 파일들 생성

  - Order

    ```java
    package hello.core.order;
    
    public class Order {
        private Long memberId;
        private String itemName;
        private int itemPrice;
        private int discountPrice;
    
        public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
            this.memberId = memberId;
            this.itemName = itemName;
            this.itemPrice = itemPrice;
            this.discountPrice = discountPrice;
        }
    
        public Long getMemberId() {
            return memberId;
        }
    
        public void setMemberId(Long memberId) {
            this.memberId = memberId;
        }
    
        public String getItemName() {
            return itemName;
        }
    
        public void setItemName(String itemName) {
            this.itemName = itemName;
        }
    
        public int getItemPrice() {
            return itemPrice;
        }
    
        public void setItemPrice(int itemPrice) {
            this.itemPrice = itemPrice;
        }
    
        public int getDiscountPrice() {
            return discountPrice;
        }
    
        public void setDiscountPrice(int discountPrice) {
            this.discountPrice = discountPrice;
        }
    
        @Override
        public String toString() {
            return "Order{" +
                    "memberId=" + memberId +
                    ", itemName='" + itemName + '\'' +
                    ", itemPrice=" + itemPrice +
                    ", discountPrice=" + discountPrice +
                    '}';
        }
    }
    
    ```

    Constructer, Getter,Setter, toString 생성

    + toString : 출력할때 기본으로 나오는 형식이 됨

  - OrderService(interface)

    ```java
    package hello.core.order;
    
    public interface OrderService {
        Order createOrder(Long memberId, String itemName,int itemPrice);
    }
    ```

  - OrderServiceImpl(class)

    ```java
    package hello.core.order;
    import hello.core.discount.DiscountPolicy;
    import hello.core.discount.FixdiscountPolicy;
    import hello.core.member.*;
    
    public class OrderServiceImpl implements OrderService{
        private final MemberRepository memberRepository=new MemoryMemberRepository();
        private final DiscountPolicy discountPolicy=new FixdiscountPolicy();
    
        @Override
        public Order createOrder(Long memberId, String itemName, int itemPrice) {
            Member member= memberRepository.findById(memberId);
            int discountPrice = discountPolicy.discount(member,itemPrice);//할인율
    
            return new Order(memberId,itemName,itemPrice,discountPrice);
        }
    }
    ```

  이렇게 하면 주문과 할인 도메인 개발도 완성이다. 이제 만든 도메인을 테스트해보자.

  참고 : 김영한 - '스프링 핵심 원리 - 기본편'
