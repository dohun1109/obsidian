---
banner: https://gongu.copyright.or.kr/gongu/wrt/cmmn/wrtFileImageView.do?wrtSn=11288959&filePath=L2Rpc2sxL25ld2RhdGEvMjAxNS8wMi9DTFM2OS9OVVJJXzAwMV8wNDQ1X251cmltZWRpYV8yMDE1MTIwMw==&thumbAt=Y&thumbSe=b_tbumb&wrtTy=10006
---
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



---
# @Configuration과 바이트코드 조작의 마법 

- 스프링 컨테이너는 싱글톤 레지스트리다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다. 그런데 스프링이 자바 코드까지 어떻게 하기는 어렵다. 위 자바 코드를 보면 분명 3번호출 되어야 하는 것이 맞다. 그래서 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다. 모든 비밀은 @Configuration을 적용한 AppConfig에 있다. 

즉, @Configuration 어노테이션이 붙으면 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것이다! 

그 임의의 다른 클래스가 싱글톤이 보장되도록 해준다. 아마도 다음과 같이 바이트코드를 조작해서 작성되어 있다. (실제로는 CGLIB 내부 기술을 사용하는데 매우 복잡하다.)


>[!NOTE] **AppConfig@CGBLB 예상 코드 **
> ``` java  
> 	@Bean 
> 	public MemberRepository memberRepository(){
> 		if(memoryMemberRepository가 이미 스프링 컨테이너에 등록되어 있으면?){
> 			return 스프링 컨테이너에서 찾아서 반환;
> 		}else{ //스프링 컨테이너에 없으면 
> 			기존 로직을 호출해서  MemoryMemberRepository 를 생성하고 스프링 컨테이너에 등록 
> 			return 반환 
> 		}
> 	}
> ```

- @Bean 이 붙은 메서드 마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 비능로 등록하고 반환하는 코드가 동적으로 만들어 진다.
- 덕분에 싱글톤이 보장되는 것이다. 
  
>[!TIP] 참고 
> AppConfig@CGLIB는 AppConfig의 자식타입으로 , AppConfig 타입으로 조회 할 수 있다. 

>[!Warning]  **정리**
> - @Bean 만 사용해도 스프링 빈으로 등록 되지만, 싱글톤을 보장하지 않는다 .
> 	- 'memberRepository' 처럼 의존관계주입이 필요해서 메서드를 직접 호출할 때 싱글톤을 보장하지 않는다
> - 크게 고민할 것이 없다. 스프링 설정 정보는 항상 '@Configuration' 을 사용하자 !!!!


[[컴포넌]]