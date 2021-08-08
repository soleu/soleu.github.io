---

title:  "[Spring Study]  - 테스트 케이스 작성(2) "
excerpt: "테스트 케이스 실행하기"

categories:

  - Spring
    tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true

date: 2021-08-05
last_modified_at: 2020-08-05

---

테스트 케이스 실행하기

- 테스트 코드는 빌드에 포함되지 않음.

- main에서 테스트 케이스를 작성한다.

ctrl + shift + T : create test

![image-20210806023558436](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210806023558436.png)

원하는 메서드를 선택하면, test package에 알아서 만들어 준다.

  - test 작성법

    - given , when, then 의 형식으로 작성한다.

      ```java
          @Test
          void 회원가입() {
      //given : 뭔가가 주어졌는데
              Member member = new Member();
              member.setName("hello");
              //when : 이걸 실행했을 때
              Long saveId =memberService.join(member);
      
              //then : 이런 결과가 나와야한다.
              Member findMember=memberService.findOne(saveId).get();
              assertThat(member.getName()).isEqualTo(findMember.getName());
          }
      ```

```java
   public void 중복_회원_예외(){
        //given
        Member member1=new Member();
        member1.setName("spring");

        Member member2=new Member();
        member2.setName("spring");
	  //when : 이런식으로 try-catch문을 써도 되지만, 비효율적
        try {
            memberService.join(member2);//validate에서 중복 예외 되어야함.
            fail();//에러 : 실패했는데 예외가 발생하지 않음
        }catch (IllegalStateException e){
            //정상흐름 : 예외 발생
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.23");//실패
        }
    }
```

```java
        //when
        memberService.join(member1);
        //콤마 뒤를 실행할건데, 에러가 나면 에러코드를 출력해라
        IllegalStateException e = assertThrows(IllegalStateException.class,()-> memberService.join(member2));
		//문자열 비교
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

```

when 문에서 try-catch문을 사용해도 되지만, 밑처럼 spring의 문법을 사용하여 표현하는게 효율적이다.
