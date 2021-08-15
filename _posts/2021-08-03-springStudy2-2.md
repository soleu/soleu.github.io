---
title:  "[Spring Study]  -회원 관리 예제(2)"
excerpt: "회원 도메인과 리포지토리 만들기"

categories:
  - Spring
tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true
 
date: 2021-08-03
last_modified_at: 2021-08-03
---

**Spring Study**  - 회원 관리 예제(2) - 회원 도메인과 리포지토리 만들기



***Optional***

: findById,findByName의 값이 null 일 경우 처리 하는 방법 

-> Optional로 감싸서 null 을 반환(자바 8 update)



![image-20210803025729222](https://raw.githubusercontent.com/soleu/image_repo/main/img/image-20210803025729222.png)

[회원 도메인]

domain - Member  : member 클래스 객체 정의

```
package hello.hellospring.domain;

public class Member {
    private Long id;//System id
    private String name;

    public Long getId(){
        return id;
    }

    public void setId(Long id){
        this.id=id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

[레포지토리] : 저장소

repository - MemberRepository : 저장소 클래스 객체 정의

```
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findById(Long id); //id로 반환
    Optional<Member> findByName(String name);//이름으로 반환
    List<Member> findAll();//전체 반환
}
```

repository -MemoryMemberRepository : 저장소 객체 로직

```
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import javax.swing.text.html.Option;
import java.util.*;

public class MemoryMemberRepository implements MemberRepository {
    private static Map<Long,Member>store=new HashMap<>();
    private static long sequence =0L;//0,1,2..key값 생성

        @Override
        public Member save(Member member) {
            member.setId((++sequence));//id 생성
            store.put(member.getId(),member);//store에 저장
            return  member;
        }

        @Override
        public Optional<Member> findById(Long id) {
            //이런식으로 optional로 감싸면, 클라이언트에서 후처리가능
            return Optional.ofNullable(store.get(id));
        }

        @Override
        public Optional<Member> findByName(String name) {
            return store.values().stream() //loop
                    .filter(member -> member.getName().equals(name)) //name 일치 확인
                    .findAny();//하나라도 찾으면 반환.없으면 null
        }

        @Override
        public List<Member> findAll() {
            return new ArrayList<>(store.values());
        }
    }

```





