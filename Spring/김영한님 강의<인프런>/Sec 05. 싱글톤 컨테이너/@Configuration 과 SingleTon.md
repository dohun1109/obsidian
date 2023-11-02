# Spring Study  <Configuration과 싱글톤 >****
- ### @Configuration 은 싱글톤을 위해 만들어 졌다 ! 

``` java
	@Configuration
 public class AppConfig {

     @Bean     
     public MemberService memberService() {

         return new MemberServiceImpl(memberRepository());
     }

     @Bean     
     public OrderService orderService() {

         return new OrderServiceImpl(
                 memberRepository(),

                 discountPolicy());

}

     @Bean    
      public MemberRepository memberRepository() {

         return new MemoryMemberRepository();
     }

}
```

위의 코드에서  memberService 와 orderService 를 호출하게 되면 순수 자바코드로만 봤을때 
MemoryMemberRepository 가 2번 이상 생성되면서 싱글톤이 깨지는 것 처럼 보이는데 

실제로도 그런지 확인해 보았다. 

> [!TIP] 예상 결과 
>>1. 스프링 컨테이너가 스프링 빈에 등록하기 위해 @Bean이 붙어있는 `memberRepository()` 호출
>>3. memberService() 로직에서 `memberRepository()` 호출
  >>3. orderService() 로직에서 `memberRepository()` 호출

이렇게 총 3번 생성된다고 생각했다 . 
그런데 출력 결과 AppConfig에서 모두 1번 씩만 호출 되었다 .

> [!WARNING] 출력 결과 
>>- call AppConfig.memberService
>>- call AppConfig.memberRepository
>>- call AppConfig.orderService


