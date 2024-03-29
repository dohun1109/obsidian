>[!WARNING] 개발자 속담 
> "내가 '객체 지향' 이란 용어를 고안했으니 말인데, C++를 염두에 두진 않았다고 말할 수 있다."
>  - Alan Kay (객체지향 프로그래밍의 아버지)


# 힙 오염 (Heap pollution)

---
힙 오염은 단어 그대로 JVM의 힙 메모리 영역에 저장되어있는 특정 변수(객체)가 불량 데이터를 참조함으로써, 만일 힙에서 데이터를 가져오려고 할 때 얘기치 못한 런타임 에러가 발생할 수 있는 오염 상태를 일컫는다. 
힙 오염의 대표적인 원인 주자로 꼽히는 것이 바로 [[제네릭(Generics)]] 이다. 
사실 Java5 버전에 제네릭 문법이 처음 도입되었을때 약간의 논란이 있었다. 왜냐하면 기존 컬렉션 프레임 워크 (Collection Framework)의 클래스들을 [raw type](https://donghyeon.dev/이펙티브자바/2021/03/25/raw-타입은-사용하지-말자/) 으로서 써왔는데 갑자기 타입 체크(type check)기능을 넣으니 기존 프로그램과의 호환성을 고려해야 하는 문제점이 있었기 때문이다. 그래서 제네릭은 컴파일하는 시점까지만 유효하고, 컴파일이 되면 제네릭 타입 파라미터를 Object 로 변환하거나 제거함으로써 하위 호환서을 맞출 수 있게 되었다. 
[[자바 제네릭 타입 소거 컴파일 과정]]

그러나 이러한 타입 소거(type erasure) 특성과 컴파일러의 특성이 합쳐지며 괴상한 표현식으로 인해 괴현상이 발생할 수 있는 잠재적인 위험성이 나오게 됬는데 그것이 heap Pullution 인것이다. 

# 제네릭 힙 오염 과정 

자바 제네릭 코드를 사용하면서 힙이 오염되는 상황은 다음 두가지가 있다. 
1. 원시 타입과 매개변수 타입을 동시에 사용하는 경우 
2. 확인되지 않는 형변환을 수행하는 경우 

다음은 잘못된 자바의 제네릭 사용 로직의 예시이다. 코드를 실행해보면 ClassCastException 예외가 발생한다. 

```java 
ArrayList<String> list1 = new ArrayList<>();
list1.add("홍길동");
list1.add("임꺾정");

// 로직 수행...
Object obj = list1;
// 로직 수행...

ArrayList<Double> list2 = (ArrayList<Double>) obj;
list2.add(1.0);
list2.add(2.0);

System.out.println(list2); // [홍길동, 임꺾정, 1.0, 2.0]

for(double n : list2) {
    System.out.println(n);
}

```

- String 서브 타입의 ArrayList를 선언하고 타입에 걸맞는 문자열 데이터를 리스트에 추가하였다.
- 그런데 잠시 최상위 타입으로써 다룰 일이 생겨서 ArrayList< String > 자체를 Object 타입으로 업캐스팅 하였다.
- 다시 ArrayList 객체로 되돌리려고 할때 개발자가 실수로 ArrayList의 서브 타입을 Double로 다운캐스팅을 해버렸다. 그리고 그대로 소수값을 리스트에 추가하였다. (**Heap 오염 !!**)
- list2 의 요소들을 출력하기 위해 double 형으로 요소들을 받아 for문을 돌린다.
- 그러나 list2 객체에는 문자열값과 소수값이 혼재되어 있어 타입 불일치로 ClassCastException 예외가 발생하게 된다.

사람 입장에선 `ArrayList<String>` 이었다가 `ArrayList<Double>` 이었다가 하는 요상하고 잘못된 코드인 것을 알아챌수 있겠지만, 컴파일러는 위의 코드 대해 어떠한 컴파일 에러를 알려주지 않는다. 
자바 엔진이 맛이 갔나 싶겠지만, 사실 이러한 현상의 원인은 제네릭의 타입 소거에 의해 나타난는 타당한 이유이기 때문이다. 

# 1. 컴파일러는 타입 캐스팅을 검사하지 않는다!!

많이들 착각하는 부분이 타입 캐스팅에 대해 컴파일러가 꼼꼼하게 체크하는 줄 안다는 것이다. 
컴파일러는 형변환 대상 객체에 대해 검사하지 않는다 . 정확히 말하면 **캐스팅 하였을때 대입되는 변수에 저장 할 수 있느냐만** 검사 할 뿐이다.
사람이 보기에는 위의 코드에서 `ArrayList<String> -> Object -> ArrayList<Double> `이라는 이상한 업 다운 캐스팅이 되었지만, 컴파일러는 list2 변수에 캐스팅한 obj 객체가 대입될수 있는지만 보기 때문에 이러한 논리적인 오류를 잡지 못하는 것이다. 
` ArrayList<Double> list2 = (ArrayList<Double>) obj; `

# 2. 제네릭 탕비 소거되면 결국 Object 
컴파일 되면서 타입이 소거되면 결구 제네릭의 타입 파리미터들은 Object로 변환되거나 제거되게 된다. 따라서 위의 코드를 컴파일하게 되면 그대로 raw type 이 되어버려 어느 타입의 데이터든 저장 할 수 있는 상태가  되기 때문에, 리스트에 문자열값과 소수값이 공존이 가능한 것이다.
```java
ArrayList list1 = new ArrayList();
list1.add("홍길동");

//로직 수행 ... 
Object obj = list1;
//로직 수행...

ArrayList list2 = (ArrayList) obj;
list2.add(1.0);

System.out.println(list2); // [홍길동, 1.0]



```

# 제네릭 힙 오염 방지책 
그러면 결구 개발자가 한땀 한땀 따져가며 조심히 제네릭을 설계 하는 수 밖에 없고 컴파일러의 도움은 받을 수 없는 것일까 ? 
이에 대해 자바에서는 Collections 클래스의 `checkList()` 메서드를 지원한다. 이 메서드는 해당 객체에 대해 의도치 않은 타입의 데이터가 들어 갔을 때 이를 감지하여 예외를 발생시켜 준다. 그래서 의도치 않은 타입의 데이터가 들어갔을 때 이를 감지하여 예외를 발생시켜 준다.그래서 문자열과 소수가 혼재되어있는 상태가 되는 힙 오염이 되는 상황 자체를 미연에 방지할 수 있게 된다. 
```java 
// 첫번째 인자 : 리스트 객체 
// 두번째 인자 : 리스트 타입 파라미터의 Class 객체 
List<String> list1 = Collections.checkedList(new ArrayList<>, String.class);

list1.add("홍길동");

// 로직 수행 ...
Object obj = list1;
// 로직 수행...

List<Double> list2 = (List<Double>) obj;
list2.add(1.0);

System.out.println(list2);

for(double n : list2){
	System.out.println(n);
}
```