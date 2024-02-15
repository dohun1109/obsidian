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
- st


