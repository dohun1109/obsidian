인스턴스 클래스 (Instance class) : 외부 클래스의 멤버변수 선언 위치에 선언하며, 클래스의 멤버처럼 다뤄진다. 
주로 외부 클래스의 인스턴스 멤버들과 관련된 작업에 사용될 목적으로 선언된다. 

정적 클래스( Static class) : 외부 클래스의 멤버변수 선언 위치에 선언하며, 외부 클래스의 static 멤버처럼 다뤄진다. 
다만 주의할 점은 static 이라고 해서 new 생성자 초기활르 못하는 건 아니다. ! 
즉, 일반적인 static 필드 변수나 static 메서드와 달리, static 내부 클래스는 같은 static 이지만 메모리 구조나 기능이 전혀 다르다. 

지역 클래스 (local class) : 외부 클래스의 메서드나 초기화 블럭안에 선언하며, 선언된 메서드 블록 영역 내부에서만 사용될 수 있다. 

익명 클래스 (anoonymous class) : 클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스이다. 주로 클래스를 일회용으로 사용할 때 이용된다. 
```java
class Outher{
	class InstanceClass{}
	static class StaticInner{}
	void method1(){
		class LocalInner{}
	}
}
```

## 인스턴스 클래스 
- class member 선언부에 위치하고 static 키워드가 없는 inner class 
- 외부 클래스의 멤버로 취급되기 때문에 외부 클래스의 객체 먼저 생성 한 후 내부 클래스의 객체를 생성이 가능하다. 
- 인스턴스 클래스 내부에는 instance 멤버만 선언가능(static member 선언 불가 )
- 주로 외부 클래스의 인스턴스 멤버들과 관련된 작업에 사용될 목적으로 선언된다. 
- 단, final static 은 상수의 역활로 선언 가능 .
- 정규화된 this -> outherclass와 innerclass에 중복된 부분이 있을때 innerclass에서 클래스명.this.메소드(멤버) 로 접근할 수 있다.
- 그리고 컴파일 시 외부클래스$내부 클래스 .class 형태로 저장된다. 
  
## 정적 내부 클래스 (Static inner class)
- static 키워드가 붙은 내부 클래스  
- 단, 일반적인 static field 나 static method 와 다르다. 
- static inner class 의 내부에는 instance member 나 static member 모두 선언 할 수 있다. 
- 그러나 일반적인 static method와 동일하게 외부 클래스의 인스턴스 멤버에는 접근이 불가하고, static 멤버에만 접근할 수 있다. 

## Static class 에 대한 오해 

static 내부 클래스가 그 외의 static 필드 변수나 static method와 같이, 'static 이니까 메모리에 하나만 올라가는 인스턴스로' 로 착각한다는 점.
이들을 각각 메인 메소드에서 변수로 받아 같은지 다른지 비교해보면, static 변수는 우리의 예상대로 메모리에서 한번만 생성 되니까 반환 되는 데이터는 같다.

하지만 내부 static 클래스는 일반 클래스처럼 초기화를 할때마다 다른 객체가 만들어짐을 볼 수 있다. (static 인데 말이다)

## 결론) 내부 클래스는 Static Inner Class 로 선언 하자 ! 
- 만일 내부 클래스를 이용하는데, 내부 클래스에서 바깥 외부의 인스턴스를 사용할 일이 없다면 static 클래스로 선언해주어야 한다. 왜냐하면 static이 아닌 내부 인스턴스 클래스는 외부와 연결이 되어 있어 '외부 참조'를 갖게 되어 메모리를 더 차지하고, 느리며, GC 대상에서 제외 되는 문제점을 일으키기 때문이다. 

-> 내부 클래스는 static으로 선언 - 아직 안봄 ..




