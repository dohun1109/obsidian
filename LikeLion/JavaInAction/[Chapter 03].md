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
Predicate<String> nonEmptyStrinPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);

```
 
 Predicate 인터페이스의 자바독 명세를 보면 and 나 or 같은 메서드도 있음을 알수 있다. 
### Consumer 
java.util.function.Consumer 인터페이스는 제네릭 형식 T 객체를 받아서 void를 반환하는 accept 라는 추상메서드를 정의한다. 
`T형식의 객체를 인수로 받아서 어떤 동작을 수행하고 싶을때 Consumer 라는 인터페이스를 사용할 수 있다.`예를 들어 Integer 리스트를 인수로 받아서 각 항목에 어떤 동작을 수행하는 forEach 메서드를 정의할 때 Consumer를 활용할 수 있다. 
```java
 @FunctionalInterface
public interface Consumer<T> {
  void accept(T t);
}

public <T> void forEach(List<T> list, Consumer<T> c) {
  for(T t : list) {
    c.accept(t);
  }
}

forEach( 
  Arrays.asList(1,2,3,4,5),
  (Integer i) -> System.out.println(i)
);
```

### Function 
java.util.function.Function<T,R> 인터페이스는 제네릭 형식 T를 인수로 받아서 제네릭 형식 R객체를 반환하는 추상메서드 apply를 정의한다. 
입력을 출력으로 매핑하는 람다를 정의할 때 Function 인터페이스를 활용할 수 있다. (예를 들면 사과의 무게정보를 추출하거나 문자열을 길이와 매핑) .
다음은 String 리스트를 인수로 받아 각 String 길이를 퐇ㅁ하는 Integer 리스트로 변환하는 map 메서드를 정의하는 예제이다. 
```java 
@FunctionalInterface
public interface Function<T, R> {
  R apply(T t);
}

public <T, R> List<R> map(List<T> list, Function<T, R> f) {
  List<R> result = new ArrayList<>();
  for(T t: list) {
    result.add(f.apply(t));
  }
  return result;
}

// [7,2,6]
List<Integer> l = map(Arrays.asList("lambdas", "in", "action"), (Strin s) -> s.length()); // Function의 apply 메서드를 구현하는 람다
```

### 구현 방식 
1. 함수형 인터페이스를 선언한다. 
2. 이 함수형 인터페이스를 동작 파라미터화 시켜서 자신이 원하는 로직을 처리하는 메서드를 만든다. 
3. 해당 메서드를 호출할 때 파라미터로 람다를 넘길수 있다. (여기서 람다는 함수형 인터페이스의 메서드를 구현하는 역활)

### 기본형 특화 
자바의 모든 형식은 참조형 아니면 기본형인데 자바에서 기본형을 참조형으로 변경하는 기능을 제공한다. 이를 boxing 이라고 한다. 반대로 참조형을 기본형으로 반환하는 동작을 unboxing 이라고 한다. 이러한 동작이 자동으로 일어나는 것을 오토박싱이라고 한다. 

```java
 List<Integer> list = new ArrayList<>();
 for(int k = 0; k<500; k++){
	list.add(k); //int -> Integer 
 }
```
하지만 이러한 변환과정은 비용이 소모된다. boxing 한 값은 기본형을 감싸는 래퍼이며 힙에 저장된다. 따라서 박싱한 값은 메모리를 더 소비하여 기본형을 가져올때도 메모리를 탐색하는 과정이 필요하다. 

### 형식 검사, 형식 추론, 제약 
람다 표현식을 처음 설명할 때 람다로 인터페이스의 인스턴스를 만들 수 있다고 언급했다. 
람다 표현식 자체에는 람다가 어떤 함수형 인터페이스를 구현하는지의 정보가 포함되어있지 않다. 따라서 람다 표형식을 더 제대로 이해하려면 람다의 실제 형식을 파악해야 한다. 

### 형식 검사 
람다가 사용되는 `콘텍스트(context)`를 이용해서 람다의 `형식(type)`을 추론할 수 있다. 어떤 콘텍스트에서 기대되는 람다 표현식의 형식을 `대상 형식(target type)`이라고 한다. 
### 형식 추론 
자바 컴파일러는 람다 표현식이 사용된 콘텍스트(대상 형식)를 이용해서 람다 표현식과 관련된 함수형 인터페이스를 추론한다.
```java
대상 형식 = () -> ...'
```


## 자유변수 (free variable)와 람다 캡처링(capturing lambda)
지금까지 살펴본 모든 람다 표현식은 인수를 자신의 바디 안에서만 사용했다. 하지만 람다 표현식에서는 익명 함수가 하는 것처럼 `자유 변수`(파라미터로 넘겨진 변수가 아닌 외부에서 정의도니 변수)를 활용할 수 있다. 이와같은 동작을 `람다 캡처링`이라고 한다. 
다음은 portNumber 변수를 캡처하는 람다 예제이다.

```java
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);
```

하지만 자유 변수에도 제약이 있다. 람다는 인스턴스 변수와 정적 변수를 자유롭게 캡처(자신의 바디에서 참조할 수 있도록) 할 수 있다.

하지만 그러러면 `지역 변수는 명시적으로 final로 선언되어 있어야 하거나, 실질적으로 final로 선언된 변수와 똑같이 사용되어야 한다.`

아래는 portNumber에 값을 두 번 할당하므로 컴파일 할 수 없는 코드이다.

```java
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);
portNumber = 31337;
```

## 클로저(Clojure)

원칙적으로 클로저란 함수의 비지역 변수를 자유롭게 참조할 수 있는 함수의 인스턴스를 가리킨다.예를 들어 클로저를 다른 함수의 인수로 전달 할 수 있다.클로저는 클로저 외부에 정의된 변수의 값에 접근하고, 값을 바꿀 수 있다. 자바 8의 람다와 익명 클래스는 클로저와 비슷한 동작을 수행한다.람다와 익명 클래스 모두 메서드의 인수로 전달 될 수 있으며, 자신의 외부 영역의 변수에 접근할 수 있다. 다만 람다와 익명 클래스는 람다가 정의된 메서드의 지역 변수의 값은 바꿀 수 없다. 람다가 정의된 메서드의 지역 변숫값은 final 변수여야 한다. 덕분에 람다는 변수가 아닌 값에 국한되어 어떤 동작을 수행한다는 사실이 명확해진다. 이전에도 설명한 것 처럼 지역 변숫값은 스택에 존재하므로 자신을 정의한 스레드와 생존을 같이 해야 하며 따라서 지역 변수는
final 이어야 한다. 가변 지역 변수를 새로운 스레드에서 캡처할 수 있다면, 안전하지 않은 동작을 수행할 가능성이 생긴다.(인스턴스 변수는 스레드가 공유하는 힙에 존재하므로 특별한 제약이 없다.)

