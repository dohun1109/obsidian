
# 변수의 기본형 & 참조형 타입 

---
## 기본형 타입 (Primitive Type)
기본형 타입에는 크게 논리형 (boolean), (문자, 정수)형 (char), 정수형 (byte, short, int, long) 실수형 (float, double) 으로 나뉜다. 

**특징** 
- 변수의 선언과 동시에 메모리 생성 
- 비 객체 타입이므로 null 값을 가질 수 없다. (기본값이 정해저 있음) nullException
- 모든 값 타입은 메모리의 스택(Stack)에 저장됨.
- 저장 공간이 실제 값을 가진다. 
할당 메모리 크기 
boolean : 1byte 
byte : 1 byte 
short  : 2byte 
int : 4byte 
long : 8byte 
float : 4byte 
double : 8byte 
char : 2byte( 유니코드)

## long type < float type  ?  true or false 
-> True 


---
## 참조형 타입 (Reference Type)
참조형 타입은 간단히 말하면, 위의 8가지 자료형을 제외한 나머지를 말한다고 보면 된다. 
기본적으로 제공하는 클래스, 프로그래머가 스스로 만든 클래스, 배열, 열거 타입 등을 모두 참조형이라고 한다. 

**특징**
- 기본형과 달리 실제 값이 저장되지 않고, 자료가 저장된 공간의 주소를 저장한다. 
- 즉, 실제 값은 다른 곳에 있으며 값이 주소를 가지고 있어서 나중에 그 주솔르 참조해서 값을 가져 온다. 
- 메모리의 힙 (heap) 에 실제 값을 저장하고, 그 참조값(주소 값)을 갖는 변수는 스택에 저장 
- 참조형 변수는 null로 초기화 시킬수 있다. 



