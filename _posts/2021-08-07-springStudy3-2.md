---

title:  "[Spring Study]  - 스프링 특징(2) "
excerpt: "스프링 빈 등록 방법 2가지 상세 설명"

categories:

  - Spring
    tags:
  - [Spring, SpringBoot]

toc: true
toc_sticky: true

date: 2021-08-07
last_modified_at: 2020-08-07

---

- 컴포넌트 스캔 방법으로 의존 관계 설정하기

```java
  private final MemberService memberService=new MemberService(); 
```

위와 같은 방식으로 new 해주면, memberController말고 다른 컨트롤러들이 memberSerivice를 가져다 쓸수있게 된다.
 여러개 생성할 필요가 없는 친구들은 딱 하나만 생길 수 있도록 관리해야한다. 

-> DI 형식으로 작성

~~~java
```MemberController
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired 
    public MemberController(MemberService memberService){
        this.memberService=memberService;
    }
}
~~~

Autowired : 

생성자가 autuwired일때 spring이 MemberService를 가져와서 Controller에 **연결**을 시켜준다.

**주의 사항) 관련된 클래스들에도 모두 어노테이션(의존관계 주입(DI))을 해줘야한다.

~~~java
```MemberService
@Service
public class MemberService {
    //memberRepository를 생성과 동시에 내부에서 선언해줄 수 있도록 함
    private  final MemberRepository memberRepository;
	@Autowired
    //memberRepository를 DI
    public MemberService(MemberRepository memberRepository){
        this.memberRepository=memberRepository;
    }
~~~

~~~java
```MemoryMemberRepository
@Repository
public class MemoryMemberRepository implements MemberRepository {
~~~



- 자바 코드로 직접 스프링 빈 등록하기

  

  ![image-20210807004953385](C:\Users\이솔\AppData\Roaming\Typora\typora-user-images\image-20210807004953385.png)

  HelloSpringApplication 경로에 SpringConfig 클래스를 생성한다.

  

  ~~~java
  ```SpringConfig.java
  @Configuration
  public class SpringConfig {
  
      @Bean
      public MemberService memberService(){
          return  new MemberService(memberRepository());
      }
  
      @Bean
      public MemberRepository memberRepository(){
          return new MemoryMemberRepository();
      }
  
  }
  
  ~~~

  MemberService, MemberRepository Bean에 등록을 하고,
  spring bean에 등록되어 있는 MemberRepository를 MemberSerivce에 넣어준다.

  ```
  ```MemberController
  @Controller
  public class MemberController {
      //private final MemberService memberService=new MemberService(); /
      //여러개 생성할 필요가 없는 친구들은 딱 하나만 생길 수 있도록 관리함
  
      private final MemberService memberService;
  
      @Autowired
      public MemberController(MemberService memberService){
          this.memberService=memberService;
      }
  }
  ```

  Controller는 불가피하게 컴포넌트 스캔 방식을 이용한다.

  참고) 자바코드 방식을 사용하면, 기존의  추상클래스로 정의를했던 MemoryMemberRepository를 추후에 확정된 레포지토리로 변경할  때 return 값만 변경해주면 된다.
