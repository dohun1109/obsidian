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
public static List<Apple> filterGreenApples(List<Apple> inventory){
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
	List<Apple> reuslt = new ArrayList<>();
	for(Apple apple : inventory){
		if(apple.getColor().equals(color)){
			result.add(apple);
		}
	}
	return result;
}
```
이제 아래 처럼 호출하여 사용할수 있을 것이다.
```java
List<Apple> greenApples = filterApplesByColor(inventory, GREEN);
List<Apple> redApples = filterApplesByColor(inventory, RED);
```
이제 농부가 어떤 색을 원하든 해당 메서드만 호출하면, 색으로 필터링 할 수 있다. 하지만 농부가 색 이외에도 무거운 사과와 가벼운 사과, 무게로 사과를 필터링하고 싶다고 하면 어떻게 해야할까?
`색을 파라미터화` 한 것처럼 `무게를 파라미터화` 하면 된다 .
```java
public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory ){
		if(apple.getWeight() > weight){
			result.add(apple);
		}
	}
	return result;
}
```

위 코드도 좋은 해결책이지만, 각 사과에 피러링 조건을 적용하는 부분의 코드가 색 필터링 코드와 대부분 중복된다. 

이는  소프트웨어 공학의 `DRY(Don't Repeat yourself`같은 것을 반복하지 말 것 원칙을 어기는 것이다. 이를 해결하기 위해서 색과 무게를 filter 라는 메서드로 합치는 방법도 있다. 따라서 색이나 무게중 어떤것을 기준으로 필터링할지를 가리키는 플래그를 추가할 수 있다. `(하지만 실전에서는 절대 이방법을 사용하지말아야 한다.)`
### 절대 사용하지 말하야하는 방법 
```java 
public static List<Apple> filterApples(List<Apple> inventory, Color color, int weight, boolean flag){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory){
		if((flag && apple.getColor().equals(color)) || (!flag && apple.getWeight() > weight)){
		result.add(apple);
		}
	}
	return result;
}
```
위 메서드를 아래처럼 호출할 수 있다.
