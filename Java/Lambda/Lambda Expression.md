---
sticker: lucide//code-2
banner: https://gongu.copyright.or.kr/gongu/wrt/cmmn/wrtFileImageView.do?wrtSn=11288959&filePath=L2Rpc2sxL25ld2RhdGEvMjAxNS8wMi9DTFM2OS9OVVJJXzAwMV8wNDQ1X251cmltZWRpYV8yMDE1MTIwMw==&thumbAt=Y&thumbSe=b_tbumb&wrtTy=10006
tags: JAVA, Lambda Expression
---
물론, 문서의 내용을 수정해드릴 수 있습니다. 아래는 수정된 내용입니다:

## 특징

- 익명 함수(Anonymous Function)의 한 종류로, 메소드 이름이 필요 없다.
- 람다식 내에서 사용되는 Local Variable은 `final` 키워드가 붙지 않아도 상수로 간주된다.
- 람다식으로 선언된 변수명은 다른 변수명과 중복될 수 없다.

## 단점

- 람다를 사용하면서 만든 익명 함수는 재사용이 어렵다.
- 디버깅이 어렵다.
- 람다를 과용하면 비슷한 함수가 중복 생성되어 코드가 복잡해질 수 있다.
- 재귀 함수로 만들기에 부적합하다.


> [!NOTE] 배열을 반복할 때 
>  for향상문, forEach 람다식 , :: 식 사용 
> ```java
> for (var s : lists) {}
> lists.forEach((s) -> System.out.println(lists));
> lists.forEach(System.out::println);
> // 모두 같은 결과를 출력한다.
> ```

## @Functional Interface

- '@Functional Interface'는 "람다 표현식을 구현해야 하는 추상 메소드가 하나일 때 사용 가능하다"는 의미와 동시에 추상 메소드의 개수가 제한된다. 따라서 하나의 메소드만을 가져야 한다.
- Java8 이전에 사용했던 익명함수(anonymous method)들은 람다식으로 변경에 코드를 줄일 수 있게 되었고, 람다식으로 생성된 순수 함수는 함수형 인터페이스로만 선언이 가능하다는 점이다. 

> [!NOTE] 중첩 인터페이스(Nested Interface and Inner Interface)
> 중첩 인터페이스의 특징:
> - 중첩 인터페이스는 기본적으로 정적(static)이다. 따라서, static 키워드를 명시할 필요가 없다.
> - 클래스 내부에 선언된 중첩 인터페이스는 모든 접근 제한자를 사용할 수 있다.
> - 인터페이스 내부에 중첩된 인터페이스는 public 제한자만 사용할 수 있으므로 암시적으로 public이다.


## Java에서 제공하는 함수형 인터페이스

자바에서는 자주 사용될 것 같은 함수형 인터페이스가 이미 정의되어 있으며, 총 4가지 인터페이스를 지원한다:

[{선수 지식 }일급 객체에 관한 설명  ](https://inpa.tistory.com/entry/CS-%F0%9F%91%A8%E2%80%8D%F0%9F%92%BB-%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4first-class-object)
- Consumer\<T\>  -> 값을 반환하지 않는다.
- Function\<T, R\> -> 연산하고 수행결과를 반환한다.
- Predicate\<T\> -> 항상 boolean 결과를 return 한다.
- Supplier\<T\> -> 


> [!TIP]   1. Consumer\<T\>
> - Consumer는 T를 매개변수로 받아서 사용하며, 반환값은 없는 함수형 인터페이스이다. Consumer는 `void accept(T t)`를 추상 메소드로 갖는다. 또한 Consumer는 `andThen`이라는 함수를 제공하는데, 이를 통해 하나의 함수가 끝난 후 다음 Consumer를 연쇄적으로 이용할 수 있다. 그리고  메소드를 실행할 때  데이터를 return 하지는 않는다 .

> [!TIP]  2. Function<T, R> -> R apply(T t) 
 > -  Function은 객체 T를 매게변수로 받아서 처리한 후 R로 반환하는 함수형 인터페이스다.  Function 은 R apply(T t) 를 추상메소드로 갖는다. 또한 Function은 Consumer와 마찬가지로 andThen을 제공하고 있으며, 추가적으로 compose를 제공하고 있다. 앞에서 andThend은 첫 번째 함수 가 실행된 이후에 다음 함 수를 연쇄적으로 실행하도록 연결해준다고 하였다. 하지만 compose는 첫 번째 함수 실행 이전에 먼저 함수를 실행 하여 연쇄적으로 연결해준다는 점에서 차이가 있다 또한, identity 함수가 존재하는데, 이는 자기 자신을 반환하는 static method 이다. 아 그리고 @FunctionalInterface는 추상메소드가 하나일 때  가능하다고 했는데 왜 여러개의 함수가 있냐면 default method로 구현된 상태로 지원되기 떄문에 가능하다. 
 



