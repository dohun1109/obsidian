# 1.1 스프링 부트 소개 
## 1.1.1 왜 스프링 부트 인가 
스프링 프레임워크는 복잡한 EJB 개발을 다순화하자는 취지로 시작됐다. 애플리케이션 개발전략을 단순하게 만들어주고 개발 과정에서 필용한 여러 가지 고된 일을 스프링 프레임워크가 대신 해주면서 엄청난 인기를 끌게됐다. 
## 1.1.2 스프링 부트는 무엇인가?
스프링 부트는 자바 웹 애플리케이션 개발에 드는 노력을 줄이기 위해 2014년에 4월에 처음 출시됐다. 스프링 부트는 개발자가 기술적인 보일러플레이트 코드나 복잡한 설정이 아니라 비즈니스 로직에 더 집중할 수 있게 해줬다. 스프링 부트는 스프링 프레임워크에서 사용되는 미리 정의된 형식을 따름으로써 애플리케이션 개발자가 빠르게 개발 과정을 시작할 수 있게 해준다. 스프링 부트는 스프링 프레임워크와 개발자 사이에 존재하는 계층으로서 설정을 단순화해주는 역활을 한다고도 볼 수 있다. 

## 1.1.3 스프링 부트 핵심 기능 
많은 프레임워크 사이에서 스프링부트를 돋보이게 하는 주목할 만한 기능은 다음과 같다. 
- 빠른 시동 -스프링 부트의 주요 목표 중 하나는 스프링 애플리케이션 개발을 빨리 시작할 수 있게 만드는 것이다. 만약 스프링만으로 개발한다면 아래와 같은 과정을 거처야함.
	1. 스프링 MVC 의존 관계를 추가하고 메이븐이나 그레이들 프로젝트 설정 
	2. 스프링 MVC DispatcherServelet 설정 
	3. 애플리케이션 컴포넌트를 WAR 파일로 패키징 
	4. WAR 파일을 아피치 톰캣과 같은 서블릿 컨테이너에 배포 

- **자동 구성(autoconfiguration)** : 스프링 부트는 클래스패스에 있는 JAR 파일이나 여러가지 설정파일에 지정된 프로퍼티 정보를 바탕으로 스프링 에플리케이션에 필요한 최소한의 컴포넌트를 알아서 구성해준다. 
- **미리 정의된 방식(opinionted)** : 스프링 부트는 미리 정의된 방식을 따른다. 그래서 스프링 애플리케이션을 실행할 때 필요한 몇 가지 컴포넌트를 스타터 의존관계를 기준으로 자동으로 구성한다. 예를 들어 Spring-boot-starter-web 의존관계만 추가해주면 Spring-web, spring-webmvc 처럼 웹 애플리케이션 개발에 필요한 의존관계를 클래스패스에 전부 넣어준다.
- **독립 실행형 (standalone)**: 스플이 부트 애플리케이션은 웹 서버를 내장하고 있어서 외부 웹 서버나 애플리케이션 서버 없이도 독립적으로 설치되어 실행할 수 있다. 
- **실제 서비스 환경에 사용가능(production-ready)**: 스프링 부트에는 헬스 체크, 스레드 덤프를 수행하고 기타 유용한 측정 지표를 보여주는 기능이 포함돼 있어서, 실제 서비스환경에 배포된 애플리케이션 모니터링이나 유지관리를 손쉽게 수행할 수 있다. 

## 1.1.4 스프링 부트 컴포넌트 
스프링 부트는 애플리케이션 개발 특정 영역에 특화돼 있는 여러가지 컴포넌트로 구성돼 있다. 
- spring-boot : 스프링 부트의 기본 컴포넌트로서 다른 컴포넌트를 사용할 수 있도록 지원한다. 
- spring-boot-autoconfigure : 스프링 부트 애플리케이션 자동 구성 기능을 담당하는 컴포넌트로서 클래스패스와 설정 파일의 프로퍼티에 지정된 의존 관계를 바탕으로 스프링 빈을 추론해서 알맞은 빈을 생성한다. 
- spring-boot-starters : 미리 패키징된 의존관계 기술서 모음 이다. 
- spring-boot-CLI : 그루비코드를 컴파일하고 실행할 수 있는 개발자 친화적 명령행 도구로서, 파일 내용 변경을 감지하는 기능이 있어서 애플리케이션이 수정 사항이 발생할 때 마다 직접 재부팅을 할 필요가 없다. 개발자는 CLI 도구 덕분에 메이븐이나 그레이들 같은 의존관계 관리 도구로부터 벗어날 수 있다. 
- spring-boot-actuator : 스프링 부트 애플리케이션을 모니터링하고 감시할 수 있는 액추이터 엔드포인트를 제공한다. 
- spring-boot-actuator-autoconfigure : 클래스 패스에 있는 클래스를 기반으로 액추에이터 엔드포인트를 자동 구성해주는 컴포넌트로서, 예를 들어 Micrometer 의존관계가 클래스패스에 있으면 스프링 부트가 자동으로 MetricsEndpoint 를 액추에이터 엔드포인트로 추가해준다. 
- spring-boot-test : 테스트 케이스 작성에 필요한 애너테이션과 메서드가 포함되어있다. 
- spring-boot-test-autoconfigure : 테스트 케이스에 필요한 의존관계를 자동으로 구성해준다. 
- spring-boot-loader : 스프링 부트 애플리케이션을 실행 가능한 하나의 JAR 파일로 패키징하는데 필요한 것들이 모여있다. 
- spring boot devtools : 여러가지 개발을 도와주는 도구들이 포함되어있다. 
# 1.3  스프링 부트 시작하기 
## 메이븐 pom.xml 파일 
pom.xml 파일은 크게 세 가지 부분으로 구성돼있다. 
1. parent 태그 
2. dependencies 태그 
3. 스프링 부트 메이븐 플러그인 
spring-boot-start-parent 는 모든 스프링 부트 스타터 의존 관계의 부모 스타터다. spring-boot-starter-parent 를 명시하면 프로젝트가 자식 스프링 부트 프로젝트로서 부모 프로젝트의 몇가지 부분을 확장한다는 것을 나타낸다. 
Spring-boot-starter-parent는 기본 자바 버전 지정과 스프링 부트 프로젝트에서 사용되는 몇가지 메이븐 플러그인에 대한 기본 설정을 제공하는 특별한 유형의 스타터다. maven-war-plugin 과 maven-surefire-plugin 같은 플러그인이 spring-boot-starter-parent 의존관계에 포함돼 있다. 
spring-boot-starter-parent 는 의존관계 관리에도 도움을 준다. dependencies 에 나열된 의존관계에 버전 정보가 저혀 명시돼 있지 않지만 적절한 버전이 spring-boot-starter-parent 안에 명시돼 있다. 

> 프로젝트에 이미 부모 pom 이 있다면 ? 
> 이런 경우에도 스프링 부트 패어런트 pom 에서 제공하는 의존관계 관리 같은 여러 기능을 사용할 수 있다. dependecyManagement 태그 안에 다음과 같이 spring-boot-dependencies 의존관계를 명시 하면 의존관계 관리 기능을 사용할 수  있다. 

### 스프링 부트 스타터 의존관계 
스프링 부트 스타터 의존관계는 애플리케이션의 특정 컴포넌트를 개발하기위해 필요한 라이브러리를 식별하여 적절한 라이브러리와 버전을 알아내더라도 얼마지나지 않아 버전이 변경되어 구식이 돼어버리거나 선택한 라이브러리에서 의존하는 라이브러리가 있으며 이런 의존 관계 전파로 버전 문제가 복잡해지는 문제를 해결해 주었다. 이로인해 의존관계 버전관리, 업데이트 등 여러 이슈로 부터 벗어날 수 있게되었다. 

그리고 pom.xml 의 파일 끝부분에는  `spring-boot-maven-plugin` 이 있는데, 이 플러그인은  애플리케이션 관리활동을 편하게 수행할 수 있게 도와준다. **스프링 부트 애플리케이션을 아주쉽게 실행가능한 JAR 파일로 만들거나 WAR 파일로 패키징** 할 수 있는데, 이는 spring-boot-maven-plugin 의 repackage goal 덕분이다. ` Repackage 골은 메이븐이 아직 실행할 수 없는 상태의 JAR 파일이나 WAR파일을 실행가능 하도록 만들어 준다.`

현재 개발중인 스프링 부트 애플리케이션을 실행하려면 명령해이나 터미널에서 pom.xml  파일이 있는 디렉터리로 이동 후 `mvn spring-boot:run` 을 입력하면 실행된다. HTTP 8080(로컬 실행 기본 포트) 로 접근가능하다 .

## 스프링 부트 메인 클래스 
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBootAppDemoApplication{
	public static void main(Stringp[] args){
		SpringApplication.run(SpringBootAppDemoApplication.class, args);
	}
}
```
1. main() 메서드 사용 
2. @SpringBootApplication 사용
3. SpringApplication 클래스의 역할 

일반적으로 웹 애플리케이션을 실행하려면 애플리케이션 컴포넌트를 WAR 나 EAR 아카이브 파일로 빌드하고 패키징해서 톰캣 같은 웹 애플리케이션 서버에 배포해야한다. 하지만 스프링부트는 이런 과정을 상당부분 단순화 해서 WAR 나 EAR 파일을 만들 필요 없이 단순히 main() 메서드를 실행하는 전통적인 자바 애플리케이션을 실행하는 것처럼 웹 애플리케이션을 실행 할 수 있다. 

`mvn dependency:tree` 명령을 통해 애플이케이션의 의존관계 트리를 확인할 수 있다. 

메인 클래스에 `@SpringBootApplication`이 붙어 있는데 이 애너테이션은 `@EnableAutoConfiguration, @ComponentScan, @SpringBootConfiguration` 이렇게 3개의 애너테이션을 포함하고 있다. 각 애너테이션의 역활은 다음과 같다 .

- `@EnableAutoConfigu-ration` : 스프링 부트에는 `@Enable` 로 시작하는 여러가지 애너테이션이 있다. `@EnableAutoConfigu-ration`은 애플리케이션 클래스패스에 있는 JAR 파일을 바탕으로 애플리케이션을 자동으로 구성해준느 스프링부트 자동 구성 기능을 활성화 한다. 
- `@ComponentScan` : 애플리케이션에 있는 스프링 컴포넌트를 탐색해서 찾아낸다. 스프링컴포넌트는 `@Component, @Bean` 등이 붙어있는 자바 빈으로서 스프링으로 관리한다.  그리고 `@Component` 애너테이션이 붙어있으면 애너테이션에서 지정한 디렉터리와 그 하위 디렉터리를 모두 탐색해서 스프링 컴포넌트를 찾아내고 , 라이프사이클을 관리한다.`@ComponentScan`은 루트 패키지에서 시작해서 모든 하위패키지 까지 탐색한다는 점을 기억하자. 그래서 루트 패키지와 그 하위 패이지에 존재하지않는 컴포넌트는 탐색 대상에 포함되지 못하며 스프링 부트가 관리하지 못한다. 
- `@SpringBootConfiguration`: 스프링 부트 애플리케이션 설정을 담당하는 클래스에 이 애너테이션을 붙인다. 내부적으로 `@Comfiguration`을 포함하고 있어서 이 설정 클래스는 스프링 부트 컴포넌트 탐색으로 발견되며 이 클래스 안에 정의된 빈도 스프링으로 발경해 로딩된다. 결과적으로 이러한 빈을 통해 애플리케이션 설정 과정에 참여한다. 

### SpringAppllication 클래스 
이 클래스는 편히라게 스프링 부트 애플리케이션을 가동할 수 있게 해준다. 특별히 변경하지 않는한 run() 정적 메서드를 이요해서 애플리케이션을 기동하고 시작한다. 수행하는 작업은 아래와 같다. 
1. 클래스 패스에 있는 라이브러리 기준으로 ApplicationContext 클래스 인스턴스를 생성한다. 
2. CommandLinePropertySource를 등록해서 명령행 인자를 스프링 프로퍼티로 읽어 들인다. 
3. 앞의 1단계에서 생성한 ApplicationContext를 통해 모든 싱글톤 빈을 로딩한다. 
4. 애플리케이션에 설정된 ApplicationRunners 와 CommandRunners 를 실행한다. 


SpringBootApplication 클래스는 클래스 패스에 있는 JAR 의존관계를 바탕으로 ApplicationContext 인스턴스 생성을 시도한다. 스프링 부트 웹 애플리케이션은 서블릿 타입이거나 리액티브 타입이거나 둘중 하나이다. 스프링은 클래스패스에 있는 클래스를 바탕으로 어떤 타입의 webapp 인지 유추한다.  아래는 컨텍스트를 로딩할때 과정이다. 
1. 서블릿 기반 webApp 이라고 판별되면 스프링 부트는 AnnotationConfugServletWeb-ServerApplicationContext 클래스 인스턴스를 생성한다. 
2. 리액티브 기반 웹 애플리케이션이라고 판별되면 스프링 부트는 AnnotationConfigReactiveWebServerApplicationContext 클래스 인스턴스를 생성한다. 
3. 서블릿도 아니고 리액티브도 아니면 스프링 부트는 AnnotationConfigApplicationContext 클래스 인스턴스를 생성한다. 

그리고 개발자가 SpringApplication 인스턴스에 직접 웹 애플리케이션 타입을 지정해 줄수도 있다. 

### 애플리케이션 설정 정보 
application.properties 파일을 이용해서 key=value 형태로 설정정보를 저장한다. (YAML 형식으로도 사용가능하다.)

## JAR 파일 구조 
- META-INF : 실행할 JAR 파일에 대한 핵심정보를 담고 있는 MANIFEST.MF 파일이 들어있다. 파일안에는 두가지 중요 파라미터인 Main-Class 와 Start-Class 가 들어있다. 
- 스프링 부트 로더 컴포넌트 : 스프링 부트 러더에는 실행가능한 파일을 로딩하는데 사용하는 여러가지 로더 구현체가 들어있다. JarLauncher 클래스는 JAR파일을 로딩하고 , WarLauncher 클래스는 WAR 파일을 로딩한다. 
- BOOT-INF/classes : 컴파일된 모든 애플리케이션 클래스가 들어있다. 
- BOOT-INF/lib : 의존관계로 지정한 라이브러리 들이 들어있다. 

주목 해야 될 부분은 MANIFESET.MF 파일에 있는 Main-Class 와 Start-Class 파라미터다. Start-Class는 애플리케이션이 시작할 클래스를 가리키고 , Main-Class에는 Start-Class 를 사용해서 애플리케이션을 시작하는 Launcher 클래스 이름이 지정돼 있다. 스플이 부트가 만드는 실행 가능한 JAR파일의  Start-Class 는  항상 스프링 부트 메인 클래스를 가리킨다.  

## 스프링 부트 애플리케이션 종료 
JAR 파일 명령행에서 포그라운드 프로세스로 실행하는 경우 , ctrl-c 를 눌러서 자바 프로세스로 종료하면 스프링 부트 애플리케이션도 종료되게 된다. 하지만 아무런 추가 설정없이 종료하게되면 즉시 종료되게 되며, 처리중인 요청의 처리 완료가 보장되지 않는다. 그래서 이미 처리중인 요청은 완료를 보장하는 graceful shutdown 설정이 필요하다. 
```properties
server.shutdown = graceful
spring.lifecycle.timeout-per-shutdown-phase = 1m
```

안전 종료를 설정하면 처리 중인 요청이 완료될때 까지 기다려주는 타임아웃을 설정할 수 있고 , 기본값은 30s 이다. 

## 스프링 부트 스타트업 이벤트 
스프링 프레임워크의 이벤트 관리 체계는 이벤트 발생자와 이벤트 구독자 분리를 강조한다. 
스프링 프레임워크에는 어떤 상황에 맞게 적절한 작업을 수행할 수 있도록 다양한 빌트인 이벤트가 내장돼 있다. 예를 들면 스프링 부트가 시작되고 초기화가 완료되었을 때 외부 REST API 를 호출해야되는 요구사항이 있을때 초기화 완료를 알리는 이벤트를 구독해서 외부 RESTAPI 로 호출하게 할 수 있다. 시작 및 초기화 과정에서 사용할 수 있는 빌트인 이벤트는 아래와 같다. 

- `ApplicationStatingEvent` : App 이 시작되고 리스너가 등록되면 발행된다. 스프링 부트의 LoggingSystem 은 이 이벤트를 통해 애플리케이션 초기화 단계에 들어가기전에 필요한 작업을 수행한다. 
- `ApplicationEnvironmentPreparedEvent` : App 이 시작되고 Environment 가  준비되어 검사하고 수정할 수 있게 되면 발행한다. 스프링 부트는 내부적으로 이 이벤트를 사용해 MessageConverter, ConversionService , Jackson 초기화등 여러 서비스의 사전 초기화를 진행한다. 
- `ApplicationContextInitializedEvent`: ApplicationContext 가 준비되고 ApplicationContextInitializers 가 실행되면 발행된다. 하지만 아직 아무런 빈도 로딩되지 않는다. 빈이 스프링 컨테이너에 로딩되어 초기화되기전에 어떤 작업을 수행해야 할 때 이 이벤트를 사용하면 된다. 
- `ApplicationPreparedEvent` : ApplicationContext 가 준비되고 빈이 로딩은 됐지만 아직 ApplicationContext가 리프레시되지않은 시점에 발행된다. 이 이벤트가 발행된 후에 Environment를 사용할 수 있다. 
- `ContextRefreshedEvent` : ApplicationContext가 리프레시된 후에 발행된다. 이 이벤트는 스프링 부트가 아니라 스프링이 발행하는 이벤트라서 SpringApplicationEvent를 상속하지 않는다. 스프링 부트의 ConditionEvaluationReportLoggingListener 라는 이 이벤트가 발행되면 자동 구성 보고서를 출력한다. 
- `WebServerInitializedEvent` : 웹 서버가 준비되면 발행된다. 이 이벤트는 두가지 하위 이벤트를 가지고 있는데 서블릿기반에서는`ServletWebServerInitialzedEvent`를 사용할 수 있다. 리액티브 기반은 `ReactiveWebServerInitializedEvent`를 사용가능, 이 이벤트도 SpringApplicationEvent 를 상속하지 않는다. 
- `ApplicationStartedEvent`: 애플리케이션컨텍스트가 리프레시되고나서 ApplicationRunner와 CommandLineRunner 가 호출되기 전에 발행된다. 
- `ApplicationReadyEvent`: 요청을 처리할 준비가 됐을때 SpringApplication 에 의해 발행된다. 이 이벤트가 발행되면 모든 애플리케이션이 초기화 완료된것이다. 
- `ApplicationFailedEvent` : 시작과정에서 예외가 발생되면 발행된다. 

## 스프링 부트 애플리케이션 이벤트 감지 
스타트업 과정을 소스코드로 제어할 필요가 있을때 이런 이벤트를 사용하면 편리하게 처리할 수 있다. 
예를 들어 Enviroment에 있는 파라미터를  변경해야되면 ApplicationEnvironmentPreparedEvent 를 구독하고 파라미터를 변경하면된다. 스프링 부트 애플리케이션의 여러 컴포넌트를 초기화할 때 내부적으로 이런 이벤트를 활용한다. 가장 쉬운방식은  `@EventListener`애너테이션을 사용하는 것이다. 
```java
@EventListener(ApplicationReadyEvent.class)
public void applicationReadyEvent(ApplicationReadyEvent applicationReadyEvent){
	System.out.println("Application Ready Event generated at" +new Date(applicationReadyEvent.getTimeStamp()));
}
```

`@EventListener`가 대부분의 상황에서는 잘 동작하지만 극초기에 발생되는 이벤트는 감지하지 못함으로 다른 방법이 필요하다. 

### SpringApplication 사용 
 SpringApplication 은 애플리케이션 스프트업 동작을 커스터마이징 할 수 있는 여러가지 세터 메서드도 제공한다. 예를 들어 SpringApplication 클래스가 이벤트를 감지할 수 있게 하려면 ApplicationListener 인터페이스의 onApplicationEvent() 메서드를 구현하고 이를 SpringApplication 에 추가할 수 있다.
 ```java
 public class ApplicationStartingEventListener implements ApplicationListener<ApplicationStartingEvent>{
	@Override 
	public void onApplicationEvent(ApplicationStartingEvent applicationStartingEvent){
		System.out.println("Application Starting Event logged at" + new Date(applicationStartingEvent.getTimeStamp()));
	}
 }
 
```

이 구현체를 SpringApplication 에 등록해주면 ApplicationStartingEvent 를 발행할 때 리스너를 호출한다. 
```java
@SpringBootApplication
public class SpringBootEventsApplication{
	
	public static void main(String[] args){
		SpringApplication springApplication = new SpringApplication(SpringBootEventsApplication.class);
		springApplication.addListener(new ApplicationStartingEventListener());
		springApplication.run(args);
	}

}

```

addListener(... ) 메서드는 가변인자를 통해 여러개를 한번에 받을 수도 있다. 
### spring.factories 파일을 사용한 이벤트리스너 추가 
spring.factories 파일을 이용해서 Listener 를 추가할 수 있다. (방법은 생략)


`커스텀 스프링 부트 스타터`: 애플리케이션의 의존관계를 단순화해주는 스프링부트의 핵심기능이며, 스프링 부트는 스타터 구조를 확장해서 개발자가 직접 커스텀 스타터를 만들어 활용할 수 있도록 만들어 준다 .
`커스텀 자동 구성`: 스프링 부트는 애플리케이션을 시작할 때 다양한 여러 요소를 살펴서 다양한 애플리케이션 컴포넌트를 자동으로 구성해준다.  이 자동구성 전략은 어떤 애플리케이션 컴포넌트에 대한 스프링 부트 자신만의 동작 방향을 표현할 수 있게 해주며, 스픵 부트 애플리케이션 초기화 및 실행과정에서 매우 중요한 역활을 담당한다. 
`실패 분석기` : 스프링 부트는 애플리케이션이 구동되는 과정에서 실패가 발생할때 이를 분석하고 자세한 진단 보고서를 만들어 내는 실패 분석기를 사용한다
실패 분석기도 자동 스타터나 자동 구성과 마찬가지로 개발자가 직접 커스텀 할 수 있다. 
`스프링 부트 액추에이터 ` : 스프링 부트 액추에터를 사용하면 애플리케이션을 모니터링하고 상호작용할 수 있다. 예를 들어 정상 실행 중인지를 확인하기 위해 헬스 체크를 수행한다. 그리고 여러 부분에서 분석을위해 스레드 덤프나 힙 덤프를 생성하기도 한다. 스프링 부트 액추에이터의 기본 엔드포인트는 /actuator  이며, 구체적인 지표를 뒤에 붙여서 사용한다. 기본값으로는 /health 와 /info  엔드 포인트만 HTTP 로 노출되게 된다. 
`스프링 부트 개발자 도구 `: 개발자 생산성을 높이고 개발과정을 단축하기 위해 제공한다. 그리고 개발자 도구를 사용하려면 pom.xml 에 의존관계를 추가해줘야 한다. 

