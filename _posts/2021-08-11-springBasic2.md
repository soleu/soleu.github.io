---
title:  "[Spring Basic]  - 스프링의 기초(2)"
excerpt: "객체지향 프로그래밍"

categories:
  - SpringBasic
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-11
last_modified_at: 2021-08-11
---



2. ## 객체 지향 프로그래밍

- ### 객체 지향 프로그래밍

  : 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러개의 독립된 단위, 즉 "**객체**"들의 **모임**으로 파악.

  각각의 **객체**는 **메시지**를 주고받고, 데이터를 처리할 수 있음
  -> 프로그램을 **유연**하고 **변경**이 용이하게(유지보수가 쉬움) 만들기 때문에 **대규모 소프트웨어 개발**에 많이 사용됨

  - 추상화

  - 캡슐화

  - 상속

  - **다형성**

- 다형성

  : 객체를 설계할 때 **역할**과 **구현**을 명확히 분리

  -> 객체 설계시 역할(인터페이스)을 먼저 부여하고, 그 역할을 수행하는 구현 객체 만들기

  - 인터페이스를 구현한 객체 인스턴스를 **실행 시점에 유연하게** **변경**할 수 있음
  - **클라이언트를 변경하지 않고, 서버의 구현 기능을 유연**하게 변경할 수 있음

  -> 유연하고, 변경 용이

  -> 확장 가능한 설계

  -> 클라이언트에 영향을 주지 않는 변경 가능

  -> **인터페이스를 안정적으로 잘 설계**하는 것이 중요

  

- ### 좋은 객체 지향 설계의 5가지 원칙(SOLID)

  : 클린코드로 유명한 로버드 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리
  
  - **SRP** : 단일 책임 원칙(single responsibility principle)
  
    - 한 클래스는 하나의 책임만 가져야 한다.
  
    - '변경'이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것
  
    - ex) UI 변경, 객체 생성과 사용을 분리
  
      
  
  - **OCP** : **개방-폐쇠 원칙(Open/closed principle)**
  
    - 소프트웨어 요소는 **확장에는 열려있으나 변경에는 닫혀**있어야한다.
  
    - 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현
  
      -> 구현 객체를 변경하려면 클라이언트 코드를 변경해야함(다형성 사용, OCP 원칙 위배)
  
      -> **객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요**(**DI 컨테이너의 필요성**)
  
    
  
  - **LSP** : 리스코프 치환 원칙(Liskov substitution principle)
  
    - 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야한다.
  
      
  
  - **ISP** : 인터페이스 분리 원칙(Interface segregation principle)
  
    - 특정 클라이언트를 위한 인터페이스 **여러 개**가 범용 인터페이스 하나보다 낫다.
  
    - 분리하면 인터페이스 자체가 변해도 클라이언트에 영향을 주지 않음
  
    - 인터페이스가 명확해지고, 대체 가능성이 높아짐
  
      
  
  - **DIP** : **의존관계 역전 원칙(Dependency inversion principle)**
  
    - 프로그래머는 "**추상화에 의존해야지, 구체화에 의존하면 안된다.**"
  
    - 구현 클래스에 의존 X, 인터페이스에 의존 O
  
      
  
- 다형성의 문제점

  - 다형성 만으로는 구현 객체를 변경할 때 클라이언트 코드도 함께 변경됨

    -> **OCP, DIP 위배**

  - 다형성 이외의 것으로 객체지향을 이루어 낼 수 없다.

  참고 : 김영한 - '스프링 핵심 원리 - 기본편'