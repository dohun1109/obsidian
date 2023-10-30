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
- Supplier\<T\> -> 어떤 인스턴스를 반환하는데 사용된다.


> [!TIP]   1. Consumer\<T\>
> - Consumer는 T를 매개변수로 받아서 사용하며, 반환값은 없는 함수형 인터페이스이다. Consumer는 `void accept(T t)`를 추상 메소드로 갖는다. 또한 Consumer는 `andThen`이라는 함수를 제공하는데, 이를 통해 하나의 함수가 끝난 후 다음 Consumer를 연쇄적으로 이용할 수 있다. 그리고  메소드를 실행할 때  데이터를 return 하지는 않는다 .

> [!TIP]  2. Function<T, R> -> R apply(T t) 
 > -  Function은 객체 T를 매게변수로 받아서 처리한 후 R로 반환하는 함수형 인터페이스다.  Function 은 R apply(T t) 를 추상메소드로 갖는다. 또한 Function은 Consumer와 마찬가지로 andThen을 제공하고 있으며, 추가적으로 compose를 제공하고 있다. 앞에서 andThend은 첫 번째 함수 가 실행된 이후에 다음 함 수를 연쇄적으로 실행하도록 연결해준다고 하였다. 하지만 compose는 첫 번째 함수 실행 이전에 먼저 함수를 실행 하여 연쇄적으로 연결해준다는 점에서 차이가 있다 또한, identity 함수가 존재하는데, 이는 자기 자신을 반환하는 static method 이다. 아 그리고 @FunctionalInterface는 추상메소드가 하나일 때  가능하다고 했는데 왜 여러개의 함수가 있냐면 default method로 구현된 상태로 지원되기 떄문에 가능하다. 

| **InterfaceName**        | **MethodSignature**   | **Interface Name**       | **Method Signature**    |
| -------------------- | ----------------- | -------------------- | ------------------- |
| Funcation<T, R>      | R apply(T t)      | UnaryOpearator< T >  | T apply(T t)        |
| BiFunction< T, U, R> | R apply(T t, U u) | BinaryOpearator< T > | T apply(T t1, T t2) |

 

## 클래스 멤버와 로컬 변수 사용 
- 람다식의 실행 블록에는 클래스의 멤버(필드, 메소드) 및 로컬 변수를 사용할 수 있다. 단, 블록 외부에 같은 이름의 변수를 선언하면 오류가 발생한다. 
- 람다식에서 this 는 냅적으로 생성되는 람다식을 실행한 객체의 참조이다. 


---

# 함수형 인터페이스 표준 API 
- **함수형 인터페이스(Functional interface) 는 추상메서드가 1개만 정의된 인터페이스를 통칭하여 일컫는다.
  이 인터페이스 형태의 목저은 자바에서 람다 표현식을 이용해 함수형 프로그래밍을 구현하기 위해서이다. 그런데 곰곰히 생각해 보면 함수의 형태(Signature) 는 다양하다. 함수의 리턴 값이 있을 수도 있고 없을수도 있고 매개변수 갯수가 1개 혹은 2개 여러개 일 수도 있다. 이러한 형태의 함수를 일일히 정의해서 사용하기엔 너무 양이 많으니, 자바 개발진들이 미리 함수형 인터페이스를 만드렁 제공하는 것이 바로 Functional interface 표준 API 이다. ** 

>[!TIP] TIP
> 자바에서 자료구조를 컬렉션 프레임 워크로 미리 만들어 제공하듯이, 자주 사용할 것 같은 람다 함수 형태를 함수형 인터페이스 표준 APi로 미리 만들어 제공해준다라고 이해하면 된다. 


---
# 표준 API 종류 
- 자료에 정리 된거 보다도 더 많다. 

(T : 데이터 타입 , R: 리턴 타입  -> 제네릭 타입; 더 자세한건 찾아보기(개발자들의 암묵적인 의미) )

| 함수형 인터페이스 | 메서드 형태        | API 활용                                                                                      | 매개 변수 | 반환 값 |
| ----------------- | ------------------ | --------------------------------------------------------------------------------------------- | --------- | ------- |
| Runnable          | void run()         | - 매개변수를 사용 안하고 리턴을 하지 않는 함수형태로 이용,대표적으로 쓰레드의 매개변수로 이용 | X         | X       |
| Consumer< T >     | void accept(T t)   | 매개 변수를 사용만 하고 리턴을 하지 않는 함수 형태로 이용                                     | O         | X       |
| Supplier< T >     | T get()            | 매개 변수를 사용 안하고 리턴만 하는 함수 형태로 이용                                          | O         | X       |
| Function< T, R>   | R apply(T t)       | 매개값으 매핑(=타입 변환)해서 리턴하기                                                        | O         | O       |
| Predicate< T >    | boolean test( T t) | 매개값이 조건에 맞는지 판단해서 boolean값 리턴                                                | O         | O       |
| Operator          | R applyAs(T t)     | 매개값을 연산해서 결과 리턴하기                                                               | O         | O       |

이 처럼 다양한 함수적 형태가 될 수 있는 것들을 추려 모아 정리하여 API로 제공함으로써, 우리 같은 개발자들은 굳이 추상 메소드 하나 가진 인터페이스를 일일히 정의할 필요 없이 API 만 가져다 쓰면 되는 것이다. 


---
# 표준 API는 타입 제공 용도 이다. 
 - **자바의**  