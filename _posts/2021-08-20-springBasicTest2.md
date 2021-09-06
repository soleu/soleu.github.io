---
title:  "[Spring Basic]  - 테스트 하기(2)"
excerpt: "테스트 케이스 코드 작성 방법"

categories:
  - SpringBasic
  - SpringTest
tags:
  - [Spring, SpringBoot,Test,Testcase]

toc: true
toc_sticky: true
 
date: 2021-08-20
last_modified_at: 2021-08-20
---
- ### 테스트 하기

- #### 코드를 짜며 테스트 해보기

  코드를 짜며 로직을 확인하기 위해 `Ctrl+Shift + T`를 눌러 쉽게 테스트 케이스를 생성할 수 있다.

![image-20210821215045843](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210821215045843.png)

이런식으로 테스트를 생성할 수 있는 창이 뜨는데, 별도의 수정없이 OK를 누르면 테스트 케이스를 짤 수 있다.

- #### 테스트 코드 짜는 방법

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class RateDiscountPolicyTest {
    RateDiscountPolicy discountPolicy=new RateDiscountPolicy();

    @Test
    @DisplayName("VIP는 10%할인이 적용되어야한다.")
    void vip_o(){ //성공 케이스
        //given
        Member member=new Member(1L,"member1", Grade.VIP);
        //when
        int discount= discountPolicy.discount(member,10000);
        //then
        System.out.println(discount);
        Assertions.assertThat(discount).isEqualTo(1000);
    }

    @Test
    @DisplayName("VIP가 아니면 할인이 적용되지 않아야한다.")
    void vip_x(){ //실패 케이스
        //given
        Member member=new Member(2L,"member2",Grade.BASIC);
        //when
        int discount= discountPolicy.discount(member,10000);
        //then
        System.out.println(discount);
        Assertions.assertThat(discount).isEqualTo(1000);
    }
}
```

테스트 케이스는 다음과 같이 한 부분에 대해 **성공과 실패 모두**를 테스트 해보는 것이 좋다.

그리고, 스프링 부트의 테스트 케이스에서는 `given - when - then 패턴`을 따른다.

- given : 테스트를 위해 준비하는 과정 (**변수, 입력 값 등 정의**)
- when :실제로 액션을 하는 테스트를 실행하는 과정 (**메서드 실행**)
- then : 테스트를 검증하는 과정 (예상한 값, 실제 실행을 통해서 **나온 값을** **검증**)

참고 : 김영한 - '스프링 핵심 원리 - 기본편'