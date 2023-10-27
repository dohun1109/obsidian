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
- 추가

## @Functional Interface

- '@Functional Interface'는 "람다 표현식을 구현해야 하는 추상 메소드가 하나일 때 사용 가능하다"는 의미와 동시에 추상 메소드의 개수가 제한된다. 따라서 하나의 메소드만을 가져야 한다.

> [!NOTE] 중첩 인터페이스(Nested Interface and Inner Interface)
> 중첩 인터페이스의 특징:
> - 중첩 인터페이스는 기본적으로 정적(static)이다. 따라서, static 키워드를 명시할 필요가 없다.
> - 클래스 내부에 선언된 중첩 인터페이스는 모든 접근 제한자를 사용할 수 있다.
> - 인터페이스 내부에 중첩된 인터페이스는 public 제한자만 사용할 수 있으므로 암시적으로 public이다.

## Java에서 제공하는 함수형 인터페이스

자바에서는 자주 사용될 것 같은 함수형 인터페이스가 이미 정의되어 있으며, 총 4가지 인터페이스를 지원한다:

- Supplier\<T\>
- Consumer\<T\>
- Function\<T, R\>
- Predicate\<T\>

> [!TIP]   2. Consumer\<T\>
> - Consumer는 T를 매개변수로 받아서 사용하며, 반환값은 없는 함수형 인터페이스이다. Consumer는 `void accept(T t)`를 추상 메소드로 갖는다. 또한 Consumer는 `andThen`이라는 함수를 제공하는데, 이를 통해 하나의 함수가 끝난 후 다음 Consumer를 연쇄적으로 이용할 수 있다.

> [!NOTE] 배열을 반복할 때 
>  for향상문, forEach 람다식 , :: 식 사용 
> ```java
> for (var s : lists) {}
> lists.forEach((s) -> System.out.println(lists));
> lists.forEach(System.out::println);
> // 모두 같은 결과를 출력한다.
> ```

