# 동작 파리미터화 코드 전달하기
소비자의 요구사항은 항상 바뀌기 마련이다. 이런 변화하는 요구사항에 대해 효과적으로 대응하기 위해서 `동작 파리미터화(behavior parameterizaiton)`를 이용하면 된다. 

동작 파리미터화란 아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록을 의미한다. 코드블록은 나중에 프로그램에서 호출한다. 즉, 코드블록의 실행은 나중으로 미뤄진다. 

## 예제 
> 기존의 농장 재고목록 애플리케이션에서 리스트에서 녹색(green) 사과만 필터링하는 기능을 추가한다고 가정하자. 

### 2.1.1 첫 번째 시도  : 녹색 사과 필터링 
사과 색을 정의하는 Enum 
```java
enum Color { RED, GREEN}
```
첫 번째 시도 
```java
public static List<Apple> filterGreenApples(List<Apple> inventory){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory){
		if(GREEN.equals(apple.getColor)){
			result.add(apple);
		}
	}
	return result;
}
```
만약 농부가 빨간 사과도 필터링 하고 싶다고 하면 어떻게 할까? 메서드를 하나 더 만들 수도 있지만 만약 또 농부가 노란색, 어두운 빨간색등 요구사항이 점차 늘어난다면 메서드가 많이 생길것이다. 이런경우 다음과 같은 좋은 규칙이 있다. 
> ' 거의 비슷한 코드가 반복 존재한다면 그 코드를 추상화한다. '

### 2.1.2 두 번째 시도 : 색을 파라미터화 
어떻게 해야 filterGreenApples 의 코드를 반복 사용하지 않고 filterRedApples 를 구현할 수 있을까? 색을 파라미터활 할 수 있도록 메서드에 파라미터르 추가하면 변화하는 요구사항에 좀 더 유연하게 대응하는 코드를 만들 수 있다.
```java
public static List<Apple> filterApplesByColor(List<Apple> inventory, Color color){
	List<Apple> reuslt = new ArrayList<>();
	for(Apple apple : inventory){
		if(apple.getColor().equals(color)){
			result.add(apple);
		}
	}
	return result;
}
```
이제 아래 처럼 호출하여 사용할수 있을 것이다.
```java
List<Apple> greenApples = filterApplesByColor(inventory, GREEN);
List<Apple> redApples = filterApplesByColor(inventory, RED);
```
이제 농부가 어떤 색을 원하든 해당 메서드만 호출하면, 색으로 필터링 할 수 있다. 하지만 농부가 색 이외에도 무거운 사과와 가벼운 사과, 무게로 사과를 필터링하고 싶다고 하면 어떻게 해야할까?
`색을 파라미터화` 한 것처럼 `무게를 파라미터화` 하면 된다 .
```java
public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory ){
		if(apple.getWeight() > weight){
			result.add(apple);
		}
	}
	return result;
}
```

위 코드도 좋은 해결책이지만, 각 사과에 피러링 조건을 적용하는 부분의 코드가 색 필터링 코드와 대부분 중복된다. 

이는  소프트웨어 공학의 `DRY(Don't Repeat yourself`같은 것을 반복하지 말 것 원칙을 어기는 것이다. 이를 해결하기 위해서 색과 무게를 filter 라는 메서드로 합치는 방법도 있다. 따라서 색이나 무게중 어떤것을 기준으로 필터링할지를 가리키는 플래그를 추가할 수 있다. `(하지만 실전에서는 절대 이방법을 사용하지말아야 한다.)`
### 절대 사용하지 말하야하는 방법 
```java 
public static List<Apple> filterApples(List<Apple> inventory, Color color, int weight, boolean flag){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory){
		if((flag && apple.getColor().equals(color)) || (!flag && apple.getWeight() > weight)){
		result.add(apple);
		}
	}
	return result;
}
```
위 메서드를 아래처럼 호출할 수 있다.
```java
List<Apple> greenApples = filterApples(inventory, GREEN, 0, true);
List<Apple> heavyApples = filterApples(inventory, null, 150, false);
```

하지만 정말 마음에 들지 않는 코드이다. 대체 true와 false는 무엇을 의미하는 걸까? 게다가 앞으로 요구사항이 바뀌었을때 유연하게 대처할 수 없다. 예를 들어 사과의 크기, 모양, 출하지 등으로 사과를 필터링 하고싶어지는 경우는 어떻게 해야할까? 
### 2.2 동작 파라미터화
사과의 어떤 속석에 기초해서 불리언 값을 반환(예를 들어 사과가 녹색인가? 150그램 이상인가?) . 참 또는 거짓을 반환하는 함수를 `프레디케이트`라고 한다. 선택 조건을 결정하는 인터페이스를 만들어보자 (알고리즘 패밀리 )
```java
public interface ApplePredicate{
	boolean test(Apple apple);
}
```
다음 예제처럼 다양한 선택 조건을 대표하는 여러 버전의 ApplePredicate를 정의할 수 있다. 
```java
public class AppleHeavyWeightPredicate implements ApplePredicate{
	public boolean test(Apple apple){
		return apple.getWeight() > 150;
	}
}

public class AppleGreenColorPredicate implements ApplePredicate{
	public boolean test(Apple apple){
		return GREEN.equals(apple.getColor());
	}
}
```

위 조건에 따라 filter 메서드가 다르게 동작할 것이라고 예상할 수 있다. 이를 `전략 디자인 패턴(strategy design patter)`이라고 부른다. 
전략 디자인 패턴은 각 알고리즘 (전략이라 부르는)을 캡슐화 하는 알고리즘 패밀리를 정의해둔 다음에 런타임에 알고리즘을 선택하는 기법이다. 
여기서는 ApplePredicate가 알고리즘 패밀리이며, 이를 구현한 클래스들이 전략이다. 

### 2.2.1 네 번째 시도  : 추상적 조건으로 필터링 
```java
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory){
		if(p.test(apple)){
			result.add(apple);
		}
	}
	return result;
}
```

첫 번째 코드에 비해 더 유연한 코드를 얻었으며 동시에 가독성도 좋아졌을 뿐 아니라 사용하기도 쉬워 졌다. 
예를 들어 농부가 150그램이 넘는 빨간 사과를 검색해 달라고 부탁한다면 우리는 ApplePredicate를 적절하게 구현하는 클래스만 만들면 된다. 
```java
public class AppleRedAndHeavyPredicate implements ApplePredicate{
	public boolean test(Apple apple){
		return RED.equals(apple.getColor()) &&apple.getWeight() > 150;
	}
}
```

우리가 전달한 ApplePredicate 객체에 의해 filterApples 메서드의 동작이 결정된다. 즉, 우리는 filterApples 메서드의 동작을 파라미터화 한 것이다. 
> 즉, 우리는 전략 디자인 패턴(Strategy Design Pattern) 과 동작 파라미터화를 통해서 필터 메서드에 전략(Strategy)을 전달 함으로써 더 유연한 코드를 만들었다. 

### 한개의 파라미터,  다양한 동작 
지금까지 살펴본 것처럼 컬렉션 탐색 로직과 각 항목에 적용할 동작을 분리할 수 있다는 것이 `동작 파라미터화`의 강점이다. 한 메서드가 다른 동작을 수행할 수 있도록 재활용할 수 있다.따라서 유연한 API를 만들 때 동작 파라미터화가 중요한 역활을 한다. 

### QUIZ 
사과 리스트를 인수로 받아 다양한 방법으로 문자열을 생성(커스터마이즈된 다양한 toString 메서드와 같이) 할 수 있도록 파라미터화된 prettryPrintApple 메서드를 구현하시오. 예를 들어 prettyPrintApple 메서드가 각각의 사과 무게를 출력하도록 지시할 수 있다. 혹은 각가의 사과가 무거운지, 가벼운지 출력하도록 지시할 수 있다. 

참 또는 거짓을 반환하는 함수를 `프레디 케이트`라고 했습니다. 위 문제에서 String을 반환하라고 했으니 변수명을 xxxPredicate 보다는 Formatter 라든지 이 외의 다른 좋은 네이밍으로 정하는게 좋다. (Clean Code - 의미가 분명한 이름 짓기 ) 
```java
public interface AppleFormatter {
	String accept(Apple a);

}
public class AppleFancyFormatter implements AppleFormatter{
	public String accept(Apple apple){
			String characteristic = apple.getWeight()> 150 ? "heavy" : "light";  //characteristic : 특성
		return "A " + chracteristic + " " + apple.getColor + " apple";
	}
}

public class AppleSimpleFormatter implements AppleFormatter{
	public String accept(Apple apple){
		return "An apple of " + apple.getWeight() + "g";
	}
}

public static void prettryPrintApple(List<Apple> inventory, AppleFormatter formatter){
	for(Apple apple : inventory ) {
		String output = formatter.accept(apple);
		System.out.println(output);
	}
}
```

### 2.3 복잡한 과정 간소화 
사용하기 복잡한 기능이나 개념을 사용하고 싶은 사람은 아무도 없다. 다음 예제에서 요약하는 것처럼 현재 filterApples 메서드로 새로운 동작을 전달하려면 ApplePredicate 인터페이스를 구성하는 여러 클래스를 정의한 다음에 인스턴스화해야 한다.(위에서 사용했듯 Predicate 인터페이스를 선언하고, 이를 새로운 처리과정이 필요할 때마다 구현하여 인스턴스화 하여 사용하느는것) 이는 상당히 번거로운 작업이며 시간낭비이다. 

### 2.3.1 익명 클래스 
`익명클래스`는 자바의 지역 클래스와 비슷한 개념이다. 익명 클래스를 이용하면 클래스선언과 인스턴스화를 동시에 할 수 있다. 
### 2.3.2 다섯 번째 시도 : 익명 클래스 사용
```java
List<Apple> redApples = filterApples(inventory, new ApplePredicate(){
	public boolean test(Apple apple){
		return RED.equals(apple.getColor());
	}
})
```

하지만 익명 클래스로도 아직 부족한 점이 많다. 메소드의 동작을 직접 파라미터화 하는 부분을 보W면 알 수 있는 것처럼 익명 클래스는 여전히 많은 공간을 차지한다. 

*`코드의 장황함은 나쁜특성이다. 한눈에 이해할 수 있어야 좋은 코드다 ` 

### 2.3.3 여섯 번째 시도 : 람다 표현식 사용 
```java
List<Apple> result = filterApples(inventory, (Apple apple) -> RED.equals(apple.getColor()));
```
### 2.3.4 일곱 번째 시도 : 리스트 형식으로 추상화 
```java 
public interface Predicate<T> {
	boolean test(T t);
}
public static <T> List<T> filter(List<T> list, Predicate<T> p){
	List<T> result = new AarrayList<>();
	for(T e : list) {
		if(p.test(e)){
			result.add(e);
		}
	}
	return result;
}

```

```java
List<Apple> redApples = filter(inventory, (Apple apple)-> RED.equals(apple.getColor()));
List<Integer> evenNumbers = filetr(numbers, (Integer i) -> i%2 == 0);
```

### 2.4 실전 예제 
지금까지 동작 파라미터화가 변화하는 요구사항에 쉽게 적응하는 패턴임을 확인했다. 동작 파라미터화 패턴은 동작을 (한 조각의 코드로 ) 캡슐화한 다음에 메서드로 전달해서 메서드의 동작을 파라미터화 한다. (예를 들면 사과의 다양한 프레디케이트). 자바의 API의 많은 메서드를 다양한 동작으로 파라미터화 할 수 있다. 또한 이들 메서드를 익명 클래스와 자주 사용하기도 한다. 

### 2.4.1 Comparator 로 정렬하기 
컬렉션 정렬은 반복되는 프로그래밍 작업이다. 예를 들어 처음에는 농부가 무게를 기준으로 정렬해달라고 했다가 마음을 바꿔 색을 기준으로 정렬하고 싶을수 있다. 자바8의 List 에는 sort 메서드가 포함되어 있다. (물론 Collections.sort 도 존재한다.) . 미리정의된 인터페이스인 Comparator 객체를 이용하여 sort의 동작을 파라미터화 할 수 있다. 
```java
public interface Comparator<T> {
	int compare(T o1, T o2);
}
```
Comparator 를 구현해서 sort 메서드의 동작을 다양화 할 수 있다. 에를 들어 익명클래스와 람다를 이용하여 무게가 적은 순으로 사과를 정렬할 수 있다. 
```java
inventory.sort(new Comparator<Apple>() {
	public int compare(Apple a1, Apple a2){
		return a1.getWeight().compareTo(a2.getWeight());
	}
});

inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
```

### 2.4.2 Runable 로 코드 블록 실행하기 
자바 스레드를 이용하면 병렬로 코드블록을 실행 할 수 있다. 어떤 코드를 실행할 것인지를 스레드에게 알려줄 수 있을까? 
```java
//java8 Before
public interface Runnable{
	void run();
}
Thread t = new Thread(new Runable(){
	public void run(){
		System.out.println("Hello world");
	}
})

//java8 after 
Thread t = new Thread(() -> System.out.println("hello world"));

```

### 2.4.3 Callable 을 결과로 반환하기 
자바 5 부터 지원하는 ExecutorService 인터페이스는 태스크 제출과 실행 과정의 연관성을 끊어 준다. ExecutorService 를 이용하면 태스크를 스레드 풀로 보내고 결과를 Feture로 저장할 수 있다는 점이 스레드와 Runable 을 이용하는 방식과는 다르다. 
```java
public interface Callable<V>{
	V call();
}
```
테스크를 실행하는 스레드의 이름을 반환한다. 
```java
ExecutorService executorService = Executors.newCachedThreadPool();
Future<String> threadName = executorService.submit(new     Callable<string>(){ 
	@Override
		public String call() throws Exception{
			return Thread.currentThread().getName();
		}
});
```
람다를 이용하면 아래와 같이 줄일 수 있다. 
```java
Future<String> threadName = executorService.submit(
	() -> Thread.currentThread().getName()
);
```

### GUI 이벤트 처리 
```java
// lambda 
button.setOnAction((ActionEvent event) -> label.setText("sent!!"));
```

### 마무리 
- 동작 파리미터화에서는 메서드 내부적으로 다양한 동작을 수행할 수 있도록 코드르 메서드 인수로 전달한다. 
- 동작 파라미터화를 이용하면 변화하는 요구사항을 더 잘 대응할 수 있도록 코드를 구현할 수 있으며 나중에 유지보수 비용을 줄일 수 있다. 
- 자바 API의 많은 메서드는 정렬, 스레드, GUI 처리등 다양한 동작으로 파라미터화 할  수 있다. 

