---

title:  "[Spring Study]  - 회원관리 예제(4)"
excerpt: "테스트 케이스 실행하기"

categories:

  - Spring
    tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true

date: 2021-08-06
last_modified_at: 2020-08-06

---
service 클래스 생성클래스
- service 클래스 생성
  - 클래스 이름 : **비즈니스 관련** 이름을 사용해야 함.(ex: join, findMembers)
  - ​



메서드로 바로 만들기 : ctrl + alt+ shift+T

- 같은 이름이 있는 중복 회원 분기 처리 로직

```
Optional<Member> result=memberRepository.findByName(member.getName());
result.ifPresent(member1 -> {//optional를 통한 메서드
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });//만약 값이 있다면
        //orElseGet : 값이 있을때만 꺼낸다.
       
```

이런식으로 분기 처리 가능

```
   memberRepository.findByName((member.getName()))
                  .ifPresent(m->{
                         throw new IllegalStateException(("이미 존재하는 회원입니다."));
                        });
```

이런식으로 정리 할 수 있음

위 코드와 같이, 한 로직만 있는 코드라면 메서드로 묶어도 좋음

```

```

