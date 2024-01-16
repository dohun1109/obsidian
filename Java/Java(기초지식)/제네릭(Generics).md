# 제네릭 (Generics) 이란 ? 

---
자바에서 제네릭은 **클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법**을 의미한다. 
객채별로 다른 타입의 자료가 저장될 수 있도록 한다. 
자바에서 배열과 함께 자주 쓰이는 자료형이(List)인데, 다음과 같이 클래스 선언 문법에 꺽쇠 괄호 `< >` 로 되어 있는 코드 형태를 한번 쯤은 봤을 것이다. 

```java
	ArrayList<String> list = new ArrayList<>();
```
저 **꺽쇠 괄호**가 바로 제네릭이다. 괄호 안에는 타입명을 기재한다. 그러면 저 리스트 클래스 자료형의 타입은 String 타입으로 지정되어 문자열 데이터만 리스트에 적재할 수 있게 된다. 

[아래 그림](https://inpa.tistory.com/entry/JAVA-☕-제네릭Generics-개념-문법-정복하기)과 같이 배열과 리스트의 선언문 형태를 비교해보면 이해하기 쉬울 것이다. 선언하는 키워드나 문법 순서가 다를뿐, 결국 자료형명을 선언하고 자료형의 타입을 지정한다는 점은 같다고 볼 수 있다. 

이처럼 제네릭은 배열의 타입을 지정하듯이 리스트 자료형같은 컬렉션 클래스나 메소드에서 사용할 내부 데이터입(type)을 파라미터 주듯이 외부에서 지정하는 이른바 타입을 변수화 한 기능이라고 이해하면 된다.  

>[!TIP] TIP
> 우리가 변수를 선언할 때 변수의 타입을 지정해주듯이, 제네릭은 객체에 타입을 지정해주는 것이 라고 보면 된다. 


---

## 제네릭 타입 매개변수 

위에서 보다시피, 제네릭은 `< >`꺽쇠 괄호 키워드를 사용하는데 이를 다이아몬드 연산자라고 한다. 그리고 이 꺽쇠 괄호 안에 **식별자 기호**를 지정함으로써 파라미터화 할 수 있다. 이것을 마치 메소드가 매개변수를 받아 사용하는 것과 비슷하여 제네릭의 타입 매개변수/ 타입 변수 라고 부른다. 
![[스크린샷 2024-01-11 오후 5.24.18.png]]
## 타입 파라미터 정의 
이 타입 매개변수는 **제네릭을 이용한 클래스나 메소드를 설계**할 때 사용된다. 

예를 들어 다음 코드는 제네릭을 감미한 클래스를 정의한 코드이다. 클래스명 옆에 `<T>`기호로 제네릭을 붙여준 걸 볼 수 있다. 그리고 클래스 내부에서 식별자 기호 `T`를 클래스 필드와, 메소드의 매개변수의 타입으로 지정되어 있다. 
```java 
	class FruitBox<T> {
		List<T> fruits = new ArrayList<>();

		public void add(T fruit){
			fruits.add(fruit);
		}
	}
```

제네릭 클래스를 만들었으면 이를 인스턴스화 해보자. 마치 파라미터를 지정해서 보내는 것처럼 생성 코드에서 꺾쇠 괄호 안에 지정해 주고 싶은 타입명을 할당해주면, 제네릭 클래스 선언문 부분으로 가서 타입 파라미터 `T` 가 지정된 타입으로 모둔 변호나되어 클래스의 타입이 지정되게 되는 것이다. 



## 타입 파라미터 생략 
제네릭 객체를 사용하는 문법 형태를 보면 양쪽 두 군데에 괄호 제네릭 타입을 지정함을 볼 수 있다. 하지만 맨 앞에서 클래스명과 함께 타입을 지정해 주었는데 굳이 생성자까지 제네릭을 지정해 줄 필요가 없다 (중복) 
따라서, jdk 1.7 버전 이후 부터  new 생성자 부분의 제네릭 타입을 생략할 수 있게 되었다. 제네릭 나름대로 타입 추론을 해서 생략이 된 곳을 넣어주기 때문에 문제가 없는 것이다. 


## 타입 파라미터 할당 가능 타입 
제네릭에서 할당 받을 수 있는 타입은 `Reference Type` 뿐이다. 즉, int 형이나 double형 같은 Primitive Type 을 제네릭 타입 파라미터로 넘길 수 없다는 말이다. 
우리가 Wrapper Class 에 대해 공부할 때 이미 int, double 형이 존재하는데, 왜 굳이 똑같은 역활을 하는 `Integer, Double` 형 클래스를 만들어 놨을까 고민을 해본적이 있었을 것이다. 바로 이때 사용하는 것이라고 이해하면 된다. 
OOP 에서는 모든것이 객체로 통신하기 때문에 번거롭더라도 익숙해 지어야 한다. 
```java
// 기본 타입 int는 사용불가 !!!!
List<int> intList = new List<>();

// Wrapper 클래스로 넘겨주어야 한다. (내부에서 자동으로 언박싱이 되어 원시 타입으로 이용됨)
List<Integer> intgerList = new List<>();


```

 또한 제네릭 타입 파라미터에 클래스가 타입으로 온다는 것은 , 클래스끼리 상속을 토앻 관계를 맺는 객체지향 프로그램의 **다형성**원리가 그대로 적용이 된다는 소리이다. 
 아래 예제 코드를 보면 타입 파라미터로 `<Fruit>`로 지정했지만 [업캐스팅](https://inpa.tistory.com/entry/JAVA-☕-제네릭Generics-개념-문법-정복하기) 을 통해 그 자식 객체도 할당이 됨을 볼 수 있다. 

## 복수 타입 파라미터 
제네릭은 반드시 한개만 사용하라는 법은 없다. 만일 타입 지정이 여러개가 필요할 경우 2개, 3개 얼마든지 만들 수 있다. 
제네릭 타입의 구분은 꺽쇠 괄호안에 쉼표(,)로 하며 `<T, U>`와 같은 형식을 통해 복수 타입 파라미터를 지정할 수 있다. 그리고 당연히 클래스 초기화할때 제네릭 타입을 두 개 넘겨주면 된다. 

## 중첩 타입 파라미터 
 제네릭 객체를 제네릭 탕비 파라미터로 받는 형식도 표현할 수 있다. 
 ArrayList 자체도 하나의 타입으로써(Collection) 제네릭 타입 파라미터가 될 수 있기 때문에 이렇게 중첩형식으로 사용할 수 있는 것이다. 
 ```java 
 public static void main(String[] args){
	 // LinkedList<String>을 원소로서 저장하는 ArrayList
	ArrayList<LinkedList<String>> list = new ArrayList<LinkedList<String>>();

	LinkedList<String> node1 = new LinkdedList<>():
	node1.add("aa");
	node1.add("bb");

	LinkedList<String> node2 = new LinkdedList<>();
	node2.add("11");
	node2.add("22");

	list.add(node1);
	list.add(node2);

	System.out.pritln(list); //[[aa,bb],[11,22]]

 }
```


## 타입 파라미터 기호 네이밍 
지금 까지 제네릭 기호를 `<T>` 와 같이 써서 표현했지만 사실 식별자 기호는 문법적으로 정해진 것이 없다. 
다만 우리가 for문을 이용할 때 루프 변수를 `i`로 지정해서 사용하듯이, 제네릭 표현 변수를 `T`로 표현한다고 보면 된다. 만일 두번째, 세번째 제네릭이 필요하다고 보면 for문의 `j`,`k`같이 S, U로 이어나간다 

| 타입      | 설명                              |
| --------- | --------------------------------- |
| < T >     | 타입 (Type)                       |
| < E >     | 요소 (Element), 예를 들어 List    |
| < K >     | 키 (Key), 예를 들어 Map<k, v>     |
| < N >     | 숫자 (Number)                     |
| <S, U, V> | 2번째, 3번째, 4번째에 선언된 타입 |


---
## 제네릭 사용 이유와 이점 

### 1. 컴파일 타임에 타입 검사를 통해 예외 방지 
자바에서 제네릭은 jdk1.5 버전에 추가된 스펙이다. 그래서 JDK 1.5 이전에는 여러 타입을 다ㄹ기 위해 인수나 반환값으로 Object 타입을 사용했었다. 하지만 Object로 타입을 선언할 경우 반환된 Object 객체를 다시 원하는 타입으로 일일히 타입 변환을 해야하며, 런타임 에러가 발생할 간으성도  존재하게 된다. 
아래 예제에선 Object 타입으로 선언한 배열에 Apple 과 Banana 객체 타입을 저장하고 이를 다시 가져오는 예제이다. 
```java 
	class Apple{}
	class Banana{}

	class FruitBox{
		//모든 클래스 타입을 받기 위해 최고 조상인 Object 타입으로 설정 
		private Object[] fruit;

		public FruitBox(Object[] fruit){
			this.fruit = fruit;
		}

		public Object getFruit(int index){
			return fruit[index];
		}
	}
	public static void main(String[] args){
		Apple[] arr = {
			new Apple(),
			new Apple()
		};
		FruitBox box = new FruitBox(arr);

		Apple apple = (Apple)box.getFruit(0);
		Banana banana = (Banana) box.getFruit(1);



	}
	
```

>[!WARNING] 런타임 에러(ClassCastException)
>위 같이 실행시 ClassCastException 런타임 에러가 발생하게 된다. 객체를 가져올때 형변환도 잘 해주어 문제가 없는 것 같은데 무엇이 문제일까? 
>원인은 간단하다. Apple 객체 타입의 배열을 FruitBox에 넣었는데, 개발자가 착각하고 Banana를 형변환(DownCasting)[형제간 casting 은 불가 자식A-> 부모A-> 자식A 만 가능] 하여 가져오려고 하였기 때문에 생긴 현사이다. 미리 코드에서 빨간줄로 알려줬으면 좋겠지만 보다시피 깨끗하다. **이와 같은 오류는 컴파일타임에 IDE에서 잡지 못함으로 매우 안좋은 오류이다. (런타임시 오류 발생)**

위의 코드를 제네릭의 이용하여 변경하면 런타임시 발생하던 오류를 컴파일타임에 잡을수 있으며, IDE 자체에서 바로 표시해 준다. 
```java 
	class FruitBox<T> {
		private T[] fruit;

		public FruitBox(T[] fruit){
			this.fruit = fruit;
		}

		public T getFruit(int index){
			return fruit[index];
		}
	}

	public static void main(String[] args){

		Apple[] arr = {
			new Apple(),
			new Apple()
		};
		FruitBox<Apple> box = new FruitBox<>(arr);
		Apple apple = (Apple) box.getFruit(0);
		Banana banana = (Banana) box.getFruit(1);
	}
```

이 처럼 제네릭은 클래스나 메서드를 정의할 때 타입 파라미터로 객체의 서브 타입을 지정해줌으로써, 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거하여 개발을 용이하게 해준다. 

## 불필요한 캐스팅을 없애 성능향상 

위 코드에서 다시 배열요소를 가져올때 다시 DownCasting 을 통해 가져왔다 . 이는 곧 추가적인 오버헤드가 발생하는 것과 같다. 
반면에 제네릭은 자동 형변환(type casting) 이 됨으로 굳이 casting될 type을 작성할 필요가 없다. 이는 자바에서 약간의 타입추론 기능을 제공한다는 것이다. 


# 제네릭 사용시 주의 사항 !!
##  1. 제네릭 타입의 객체는 생성이 불가 
- 당연하게도 제네릭 타입의 경우 명시적으로 정한 convention(개발자들간의 규칙, 정식 문법x ) 임으로 제네릭 타입 자체로 지정하여 객체를 생성하는 것은 불가능하다. 
  ## 2. Static 멤버에 제네릭 타입이 올 수 없다. 
- 아래 처럼 static 변수의 데이터 타입으로 제네릭 타입 파라미터가 올 수는 없다. 왜냐하면 static 멤버는 클래스가 동일하게 공유하는 변수로서 제네릭 객체가 생성되기도 전에 이미 자료 타입이 정해져 있어야 하기 때문. 즉, 제네릭 타입의 경우 인스턴스가 생성되면서 런타임에 타입이 정해진다는 것을 알 수 있다. (static 메서드 반환 타입 x , 메서드 파라미터 타입으로 x) 

## 3. 제네릭으로 배열 선언시 주의점 
>[!NOTE] 배열 선언 주의 사항!!!
> - 기본적으로 제네릭 클래스 자체를 배열로 만들 수는 없다. 

```java 
class Sample<T> {}

public class main{
	public static void main(String[] args){
		Sample<Integer>[] arr1 = new Sample<>[10];
		//java: cannot create array with '<>'
	}
}

```

하지만 제네릭 타입의 배열 선언은 허용된다. 
위의 식과 차이점은 배열에 저장할 Sample 객체의 타입 파라미터를 `Integer`로 지정한다는 뜻이다. 즉, `new Sample<Integer>()` 인스턴스는 저장이 가능하며, `new Sample<String>()` 인스턴스는 저장이 불가능하다는 소리이다. 
```java 

class Sample<T> { 
}
public class Main {
    public static void main(String[] args) {
    	// new Sample<Integer>() 인스턴스만 저장하는 배열을 나타냄
        Sample<Integer>[] arr2 = new Sample[10]; 
        
        // 제네릭 타입을 생략해도 위에서 이미 정의했기 때문에 Integer 가 자동으로 추론됨
        arr2[0] = new Sample<Integer>(); 
        arr2[1] = new Sample<>();
        
        // ! Integer가 아닌 타입은 저장 불가능
        arr2[2] = new Sample<String>();
    }
}
```


## 제네릭 메서드 (제네릭 타입 파리미터 메서드 X)
제네릭 메서드 부분은 제네릭 클래스, 인터페이스와 달리 나이도가 조금 있다. !!
아래와 같이 제네릭 클래스에서 제네릭 타입 파라미터를 사용하는 메서드를 제네릭 메서드라고 착각하기 쉬운데, 이것은 그냥 타입 파라미터로 타입을 지정한 메서드 일 뿐이다. 
```java 
class FruitBox<T>{
	public T addBox(T x, T y){ ...}
	//제네릭 클래스에서 제네릭 타입 파리미터를 사용한 메서드 예시 
	// 제네릭 메서드 X
}
```

제네릭 메서드란 , **메서드의 선언부에 `<T>` 가 선언된 메서드**를 말한다. 
위에서는 클래스의 제네릭 `<T>` 에서 설정된 타입을 받아와 반환 타입으로 사용할 뿐인 일반 메서드라면, 제네릭 메서드는 직접 메서드에 **< T >** 제네릭을 설정함 으로서 동적으로 타입을 받아와 사용할 수 있는 독립적으로 운용 가능한 제네릭 메서드라고 이해하면 된다 .

```java 
class FruitBox<T> {

	//클래스 타입 파라미터를 받아와 사용하는 일반 메서드 
	public T addBox(T x, T y){//...}

	//독립적으로 타입 할당 운영되는 제네릭 메서드 
	public static <T> T addBoxStatic(T x, T y){//...}
	


}
```


즉, 제네릭 클래스에 정의된 타입 매개변수와 제네릭 메서드에 정의된 타입 매개변수는 별개인게 되는 것이다. 제네릭 메서드의 제네릭 타입 선언 위치는 메서드 반환 타입 바로 앞이다. 


## 제네릭 메서드 호출 원리 
그럼 제네릭 메서드를 호출은 어떻게 할까? 
제네릭 타입을 메서드명 옆에 지정해줬으니, 호출 역시 메서드 왼쪽에 제네릭 타입이 위치하게 된다. 
**FruitBox. < Integer > addBoxStatic(1, 2);**

그런데 한가지 궁금한 점이 있을 것이다. 클래스 옆에 붙어있는 제네릭과 제네릭 메서드는 똑같은 `< T >` 인데 어떻게 제네릭 메서드 만이 독립적으로 운용되는지 말이다. 

사실은 처음 제네릭 클래스를 인스턴스화 하면, 클래스 타입 매개변수에 전달한 타입에 따라 제네릭 메소드도 타입이 정해지게 된다. 그런데 만일 제네릭 메서드를 호출할때 직접 타입 파라미터를 다르게 지정해주거나, 다른 타입의 데이터를 매개변수에 넘긴다면 독립적인 타입을 가진 제네릭 메서드로 운용되게 된다. 

## 제네릭 타입 범위 한정하기 
제네릭에 타입을 지정해줌으로서 클래스의 타입을 컴파일 타임에 정하여 타입 예외에 대한 안정성을 확보하는 것은 좋지만 문제는 너무 자유롭다는 점이다. 
예를 들어 다음 계산기 클래스가 있다고 하자. 정수, 실수 구분없이 모두 받을 수 있게 하기 위해 제네릭으로 클래스를 만들어 주었다. 하지만 단순히 `< T >` 로 지정하게 되면 숫자에 관련된 래퍼 클래스 뿐만 아니라 String 이나 다른 클래스들도 대입이 가능하다는 점이 문제이다. 

```java 
//숫자만 받아 계산하는 계산기 클래스 모듈 
class Calculator<T> {
	void add(T a, T b){}
	void min(T a, T b){}
	void mul(T a, T b){}
	void div(T a, T b){}
}
public class Main{
	public static void main(String[] args){
		// 제네릭에 아무 타입이나 모두 할당이 가능 
		Calculator<Number> cal1 = new Calculator<>();
		Calculator<Object> cal2 = new Calculator<>();
		Calculator<String> cal3 = new Calculator<>();
		Calculator<Main> cal4 = new Calculator<>();
	}
}
```
개발자의 의도로는 계산기 클래스의 제네릭 타입 파라미터로 Number 자료형만 들어오도록 하고 문자열이나 또 다른 클래스 자료형이 들어오면 안되게 하고 싶다고 한다. 그래서 나온 것이 **제한된 타입 매개변수(Bounded Type Parameter)** 이다. 

## 타입 한정 키워드 extends 
기본적인 용법은 < T extends \[ 제한 타입 \] > 이다. 제네릭 