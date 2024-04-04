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


