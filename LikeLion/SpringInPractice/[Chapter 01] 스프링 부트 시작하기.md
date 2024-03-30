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


