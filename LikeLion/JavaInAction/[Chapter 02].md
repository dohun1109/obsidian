# 동작 파리미터화 코드 전달하기
소비자의 요구사항은 항상 바뀌기 마련이다. 이런 변화하는 요구사항에 대해 효과적으로 대응하기 위해서 `동작 파리미터화(behavior parameterizaiton)`를 이용하면 된다. 

동작 파리미터화란 아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록을 의미한다. 코드블록은 나중에 프로그램에서 호출한다. 즉, 코드블록의 실행은 나중으로 미뤄진다. 

## 예제 
> 기존의 농장 재고목록 애플리케이션에서 리스트에서 녹색(green) 사과만 필터링하는 기능을 추가한다고 가정하자. 

### 2.1.1 첫 번째 시도  : 녹색 사과 필터링 
사과 색을 정의하는 Enum 
```java
enum Color { RED, GREEN}
```
첫 번째 시도 
```java
public static List<Apple> filterGreenApples(List<Apple inventory){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory){
		if(GREEN.equals(apple.getColor)){
			result.add(apple);
		}
	}
	return result;
}
```
만약 농부가 빨간 사과도 필터링 하고 싶다고 하면 어떻게 할까? 메서드를 하나 더 만들 수도 있지만 만약 또 농부가 노란색, 어두운 빨간색등 요구사항이 점차 늘어난다면 메서드가 많이 생길것이다. 이런경우 다음과 같은 좋은 규칙이 있다. 
> ' 거의 비슷한 코드가 반복 존재한다면 그 코드를 추상화한다. '

### 2.1.2 두 번째 시도 : 색을 파라미터화 
어떻게 해야 filterGreenApples 의 코드를 반복 사용하지 않고 filterRedApples 를 구현할 수 있을까? 색을 파라미터활 할 수 있도록 메서드에 파라미터르 추가하면 변화하는 요구사항에 좀 더 유연하게 대응하는 코드를 만들 수 있다.
```java
public static List<Apple> filterApplesByColor(List<Apple> inventory, Color color){

}
```