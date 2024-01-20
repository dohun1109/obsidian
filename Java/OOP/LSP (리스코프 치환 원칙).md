# 리스코프 치환 원칙 - LSP (Liskov Substitution Principl)
- 리스코프 치환 원칙이란? 올바른 상속 관계의 특징을 정의하기 위해 발표한 것으로, **서브 타입은 언제나 기반 타입으로 교체**할 수 있어야 한다는 것을 의미. 

교체할 수 있다는 말은, 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행이 보장되어야 한다는 의미. 

즉, 부모 클래스의 인스턴스를 사용하는 위치에 자식 클래스의 인스턴스를 대신 사용했을 때 코드가 원래 의도대로 작동해야 한다는 의미이다. 
이것을 부모 클래스와 자식 클래스 사이의 행위가 일관성이 있다고 말한다. 

무슨 심오한 논문 같이 설명하였는데, 그냥 우리가 지금까지 자바 프로그래밍을 하면서 질리도록 사용한 **다형성 원리** 를 얘기하는 것이다. 
다형성 기능을 이용하기 위해서는 클래스를 상속시켜 타입을 통합할 수 있게 설정하고, 업캐스팅을 해도 메소드 동작에 문제없게 잘 설계되어야 한다는 것이다. 

이러한 LSP 원칙을 잘 적용한 얘제가 자바의 컬렉션 프레임워크이다. 

만일 변수에 LinkedList 자료형을 담아 사용하다. 중간에 전혀다른 HashSet 자료형으로 바꿔도 `add()` 메서드 동작을 보장받기위해 Collection 이라는 인터페이스 타입으로 변수를 선언하여 할당하면 된다. 
```java 

void myData(){
	// Collection 인터페이스 타입으로 변수 선언
	Collection data = new LinkedList();
	data = new HashSet();  
	modify(data);   //method run
}
void modify(Collection data){
	list.add(1);   // 인터페이스 구현 구조가 잘 잡혀있기 때문에 add 메서드 동작이 각기 자료형에 맞게 보장됨. 
	
}


```

LSP 원칙 == 다형성을 지원하기위한 원칙 



---

# LSP 원칙 위반 예제와 수정하기
 리스코프 치환 원칙의 핵심은 **부모 클래스의 행동 규약을 자식 클래스가 위반**하면 안 된다는 것이다. 
 행동 규약을 위반한다는 것은 자식 클래스가 오버라이딩을 할 때, 잘못되게 재정의하면 리스코프 치환 원칙을 위배할 수 있다는 의미이다. 
 자식 클래스가 오버라이딩을 잘못하는 경우는 크게 두 가지로 나뉜다. 
 첫번째는 **자식 클래스가 부모 클래스의 메소드 시그니처를 자기 멋대로 변경**하거나, 두번째는 **자식 클래스가 부모 클래스의 의도와 다르게 메소드를 오버라이딩** 하는 경우가 있다. 

## 1. 자식의 잘못된 method Overriding
아래 코드처럼 Animal 클래스를 상속하는 Eagle 자식 클래스가 부모 클래스의 go() method를 자식 멋대로 코드를 재정의 한답시고 메소드 타입을 바꾸고 매개변수 갯수도 바꿔버렸다. 

한마디로 어느 메소드를 부모가 아닌 자식 클래스에서 오버로딩을 해버렸기 때문에 발생한 원칙 위반행위 이다. (그리고 애초이 이코드는 작동하지 않는다. )
```java
class Animal {
    int speed = 100;
    int go(int distance) {
        return speed * distance;
    }
}
class Eagle extends Animal {
    String go(int distance, boolean flying) {
        if (flying)
            return distance + "만큼 날아서 갔습니다.";
        else
            return distance + "만큼 걸어서 갔습니다.";
    }
}
public class Main {
    public static void main(String[] args) {
        Animal eagle = new Eagle();
        eagle.go(10, true);
    }
}
```

## 부모의 의도와 다르게 메소드 오버라이딩 
다음과 같이 Animal 클래스와 이를 상속하는 Cat 클래스가 있고, 이 동물이 포유류, 영장류, 파충류인지 출력해주는 NautralType 클래스가 있다. 
```java 
class NaturalType {
    String type;
    NaturalType(Animal animal) {
        // 생성자로 동물 이름이 들어오면, 정규표현식으로 매칭된 동물 타입을 설정한다.
        if(animal instanceof Cat) {
            type = "포유류";
        } else {
            // ...
        }
    }

    String print() {
        return "이 동물의 종류는 " + type + " 입니다.";
    }
}

class Animal {

    NaturalType getType() {
        NaturalType n = new NaturalType(this);
        return n;
    }
}

class Cat extends Animal {
}
```

