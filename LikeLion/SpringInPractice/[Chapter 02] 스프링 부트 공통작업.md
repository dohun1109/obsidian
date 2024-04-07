
## 2.1 애플리케이션 설정 관리 
개발 프로젝트를 진행하는 방식에 따라 동일한 애플리케이션을 개발, 테스트, 스테이징, 상용 환경 등 여러 환경에 배포해야하는 경우도 있다. 배포 할 환경이 달라지면 설정 정보도 달라져야 하지만, 애플리케이션 소스 코드는 거의 달라지지 않는다. 예를 들어 갭라호나겨에 사용하는 데이터베이스와 상용환겨에서 사용하는 데이터베이스가 다르므로 연결정보도 다르게 설정해야 하고 , 보안 설정도 다르게 설정해야 한다. 애플리케이션이 성장해감에 따라 더 많은 기능이 통합되고 설정 정보는 계속 늘어나면서 설정 정보 관리는 굉장히 고통스러운 일이 된다. 

스프릥 부트는 프로퍼티 파일, YAML, 환경 변수 , CLI argument 등 여러가지 방법으로 설정정보를 외부화 해서 소스코드 변경없이 환경마다 다르게 적용할 수 있다. 

### 2.1.1 SpringApplication 클래스 사용 
스프링 부트의 SpringApplication 클래스를 사용해서 애플리케이션 설정 정보를 정의할 수 있다. 이 클래스에서는 java.util.Properties 또는 java.util.Map<String, Object> 를 인자로 받는 setDefaultProperties() 메서드가 있는데 , 설정 정보를 Properties 나 Map<String, Object> 에 넣어서 이 메서드를 호출하면 설정정보가 애플리케이션에 적용된다. (이방식은 코드로 정의하는 방식으로 한 번 정의하면 나중에 바뀌지 않는 경우에 적합하다. )

properties 파일에서는 설정 정보를 포함하고 있는 파일들을 `spring.config.import` 프로퍼티를 통해 임포트해서 사용할 수 있다. `application.properties` 파일안에 `spring.config.import= classpath:additonal-application.properties` 를 추가하면 스프링 부트는 `additional-application.properties`파일이 클래스패스에 존재하지 않으면 `ConfigDataLocationNotFoundException`예외를 던진다. 

그리고 상황에 따라서 클래스파일에 설정파일이 없을 때 예외를 던지고 애플리케이션 시동 작업을 종료하는 대신에 해당파일을 무시하고 애플리케이션 시동작업을 계속 진행하게 만들어야 할 때도 있다. 이럴 때는 `spring.config.on-not-found`에 `ignore`을 지정하면 된다. 
```java 
import java.util.Properties;
//...

@SpringBootApplication
public class SpringBootAppDemoApplication{
	public static void main(String[] args){
		Properties properties = new Properties();
		properties.setProperty("Spring.config.on-not-found", "ignore");
		SpringApplication application = new SpringApplication(SpringBootAppDemoApplication.class);
		application.setDefaultProperties(properties);
		application.run(args);
	}
}
```

위 코드를 해석하면 
SpringApplication 클래스 인스턴스를 생성하고 spring.config.on-not-found에 ignore를 지정한 프로퍼티 객체를 seetDefaultProperties() 메서드의 인자로  전달하며 호출하고 있다. **이렇게 하면 spring.config.import 에 명시한 파일이 존재하지 않더라도 스프링 부트는 이 파일을 무시하고 나머지 애플리케이션 기동 작업을 진행한다.**

### @PropertySource 사용 
프로퍼티 파이르이 위치를 @PropertySource 애너테이션을 사용해서 지정할 수 도 있다.
```java
//생략 
@Configuration
@PropertySource("classpath:dbConfig.properties")
public class DbConfiguration {
	@Autowired
	private Evironment env;
	
	@Override
	public String toString(){
		return "UserName"+ env.getProperty("user")+ ", Password :" + env.getProperty("password");
	}
}
```

스프링에서 제공하는 Environment 인스턴스를 주입받으면 dbConfig.properties 파일에 지정된 설정 정보를 읽을 수 있다. dbConfiguration 클래스에서 빈을 생성하고 dbConfig.properties 에 지정된 설정정보를 읽고 출력해 보겠다.
```java
// import생략 
@SpringBootApplication
public class SpringBootAppDemoApplication {
	private static final Logger log = LoggerFactory.getLogger(SpringBootAppDemoApplication.class);

	public static void main(String[] args){
		ConfugurableApplicationContext  applicationContext = SpringApplication.run(SpringBootAppDemoApplication.class, args);
	
		DbConfiguration dbConfiguration = applicationContext.getBean(DbConfiguration.class);
		log.info(dbConfiguration.toString());
	}
}

```

### 2.1.3 환경 설정 파일 
스프링 부트는 애플리케이션 환경 설정 정보를 application.properties 또는 application.yml 파일에서 지정할 수 있다.  properties 나 yml 파일에 명시된 설정 프로퍼티 정보는 스프링의 Enviroment 객체에 로딩되고, 애플리케이션 클래스에서 Enviroment 인스턴스에 접근해서 설정 정보를 읽을 수 있으며 , @Value 애너테이션을 통해 접근할 수도 있다.

