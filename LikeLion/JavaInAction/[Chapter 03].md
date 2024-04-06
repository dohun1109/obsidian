# 람다 표현식 

## 람다(Lambda)란? 
람다 표현식은 메서드로 전달할 수 있는 익명함수를 단순화 한것. 
람다표현식은 `파라미터 + 화살표 + 바디`로 이루어진다. 
```java 
(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```
## 자바8에서 지원하는 다섯가지 람다 표현식 예제 
```java
(String s) -> s.length()
// String 형식의 파라미터 하나를 가지며 int를 반환한다. 람다 표현식에는 return이 함축되어 있으므로 return문을 명시적으로 사용하지 않아도 된다. 
(Apple a) -> a.getWeight() > 150
// Apple 형식의 파라미터 하나를 가지며 boolean(사과가 150그램보다 무거운지 결정)을 반환한다. 

(int x, int y) -> {
	System.out.println("Result:");
	System.out.println(x+y);
}
// int 형식의 파라미터 2개를 가지며 라턴값이 없다. (void return) 이 이쩨에서 볼 수 있듯이 람다 표현식은 여러 행의 문장을 포함할 수 있다. 

() -> 42 // 파라미터가 없으면 int type 42 가 return 된다. 

(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())
// Apple 형식의 파라미터 두개를 가지며 int(두 사과의 무게비교 결과를 reuturn)

```

 람다의 기본 문법 
- `(parameters) -> expression`
- `(parameters) -> { statements; }`
 람다에서 표현식이 하나인데 return 문을 사용하는  경우 블록으로 감싸야한다. 
```java
	// x
	(String s) -> return "Alan" + i;
	// o
	(String s) -> {return "Alan" + i; }
	
```

## 함수형 인터페이스 (@FunctionalInterface)
함수형 인터페이스는 `오직 하나의 추상메서드만 지정하는 인터페이스` 이다. 예를 들어 `Predicate<T>` Compartor, Runable 등이 있다. 어노테이션 `@FunctionalInterface`로 필터링을 거쳐서 추상메소드를 단 하나만 생성하도록 제한한다. (default 메서드는 제외하고 추상 메소드만 )
```java
public interface Predicate<T> {
	boolean Test(T t);
}
public interface Comparator<T> {
	int compare(T o1, T o2);
}
public interface Runable {
	void run();
}
public interface ActionListener extends EventListener {
	void actionPerformed(ActionEvent e);
}
public interface Callable<V> {   //V = value
	V call() throws Exception;
}
public interface PrivilegedAction<T> {
	T run();
}
```

람다에서 함수형 인터페이스로 뭘 할 수 있을까? 람다 표현식으로 함수형 인터페이스의 추상 메서드 구현을 직접 전달할 수 있으므로 `전체 표현식을 함수형 인터페이스 type의 인스턴스로 취급 할 수 있다.  `

## 함수 디스크립터 (funtion descriptor)
함수형 인터페이스의 추상메서드 시그니처는 람다 표현식의 시그니처를 가리킨다. 람다 표현식의 시그니처를 서술하는 메서드를 `함수 디스크립터`라고 부른다. 예를 들어 Runable 인터페이스의 유일한 메서드 run은 인수와 반화값이 없으므로 void 반환 `()-> void`로 표기할 수 있다. 

람다 표현식은 `함수형 인터페이스`를 인수로 받는 메서드에만 람다표현식을 사용할 수 있다. 
그리고 void 반환이 한 개 인 경우 중괄호를 감쌀 필요가 없다. 
## @FunctionalInterface 
@FunctionalInterface는 함수형 인터페이스에 붙는 어노테이션이다. (즉, 함수형 인터페이스를 가리키는 어노테이션) 
@FunctionalInterface로 선언했지만 실제로 함수형 인터페이스가 아니면 컴파일러가 에러를 발생시킨다. 에를 들어 추상메소드가  한 개 이상이라면 
`Multiple nonoverriding abstract methods found in interface Foo`같은 에러가 발생할 수 있다. 
## 람다 활용 : 실행 어라운드 패턴 
지원 처리( 예를 들면 데이터베이스의 파일처리)에 사용하는 `순환패턴(recurrent pattern)`은 자원을 열고 , 처리한 다음에, 자원을 닫는 순서로 이루어 진다. 설정(setup)과 정리(cleanup)과정은 대부분 비슷하다. 즉, 실제 자원을 처리하는 코드를 설정과 정리 두 과정이 둘러싸는 형태를 갖는다. 
- `실행 어라운드 패턴(execute around pattern)`
```java
public String processFile() throws IOException{
	try(BufferedReader br = new BufferedReader(new FileReader("data.txt"))){
		return br.readline();
	}
}
```
try-with-resources 구문을 사용하여 자원을 닫지 않아도 된다. 
### 1단계 : 동작(메서드) 파라미터화를 기억하라 
현재 코드는 파일에서 한 번에 한 줄만 읽을 수 있다. 만약 한 번에 두 줄을 읽거나 자주 사용된는 단어를 반환하려면 어떻게 해야 할까? 
바로 processFile() 을 동작 파라미터화 시키는 것이다. 
- 한 번에 두 줄 출력 
```java
String result = processFile((BufferedReader br)-> br.readLine() + br.readLine());
```
### 2 단계 : 함수형 인터페이스를 이용해서 동작 전달
함수형 인터페이스 자리에 람다를 사용할 수 있다. 따라서 BufferedReader -> String 과 IOException을 던질(throw)수 있는 시그니처와 일치하는 함수형 인터페이스를 만들어야 한다. 이 인터페이스를 BufferedReaderProcessor 라고 정의하자 
```java
@FunctionalInterface 
public interface BufferedReaderProcessor{
	String process(BufferedReader br) throws IOException;
}
```
```java
public String ProcessFile(BufferedReaderProcess p) throws IOException{
	//...
}
```
### 3단계 : 동작 실행 
이제 BufferedReaderProcessor에 정의된 process 메서드의 시그니처 (BufferdReader ->String) 과 일치하는 람다를 전달할 수 있다. 
람다 표현식으로 함수형 인터페이스의 추상 메서드 구현을 직접 전달할 수 있으며, 전달된 코드는 함수형 인터페이스의 인스턴스로 전달된 코드와 같은 방식으로 처리한다. 
```java
public String processFile(BufferedReaderProcessor p) throws IOException{
	try(BufferdReader br = new BufferedReader(new FileReader("data.txt"))){
		return p.process(br);
	}
}
```

### 4단계 : 람다 전달 
이제 람다를 이용해서 다양한 동작을 processFile 메서드로 전달 할 수 있다. 
```java
String oneLine = processFile((BufferedReader br) -> br.readLine());
```

## 함수형 인터페이스 사용 
자바 8 라이블러릴 설계자들은 java.util.function 패키지로 여러가지 새로운 함수형 인터페이스를 제공한다. 
### Predicate 
java.util.function.Predicate 인터페이스는 test라는 추상메서드를 정의하며 test는 제네릭 형태 T의 객체를 인수로 받아 불리언으로 반환한다. 따로 정의할 필요 없이 바로 사용할 수 있다. 
아래처럼 String 객체를 인수로 받는 람다를 정의할 수 있다. 
```java
@FunctionalInterface 
public interface Predicate<T> {
	boolean test(T t);
}

public <T> List<T> filter(List<T> list, Predicate<T> p) {
  List<T> results = new ArrayList<>();
  for(T t : list) {
    if(p.test(t)) {
      results.add(t);
    }
  }
  return results;
}
Predicat<String> nonEmptyStrinPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

