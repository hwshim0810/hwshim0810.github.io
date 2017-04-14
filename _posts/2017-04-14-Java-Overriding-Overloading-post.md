---
layout: post
title: Overriding & Overloading
description: "Difference of Overriding and Overloading"
tags: [Java, OOP]
---
# 오버라이딩(Overriding)이란?
## 정의
- Sub(자식) 클래스에서 Super(부모) 클래스에 있는 메소드와 동일한 이름으로 메소드 재작성

## 목적
- Super클래스에 구현된 메소드를 무시하고 Sub클래스에서 새로운 기능의 메소드를 재정의 하려고 할 때

## 조건
- 메소드의 이름 및 리턴 타입, 인자의 타입 및 개수 등이 모두 동일해야 함

# 오버로딩(Overloading)이란?
## 정의
- 한 클래스나 상속관계에서 동일한 이름의 메소드를 중복 작성

## 목적
- 이름이 같은 여러개의 메소드를 중복 선언하여 사용의 편리성 향상

## 조건
- 메소드 이름은 반드시 동일하며 메소드의 인자개수나 타입이 달라야 함

# 바인딩
- 오버로딩 된 메소드의 선택은 **Static** 하다
	- Compile타임에 결정됨
- 오버라이딩 된 메소드의 선택은 동적
	- Runtime에 결정된다

- 예시

```java
public class CollectionClassifier {

    public static String classify(Set<?> s) {
        return "Set";
    }

    public static String classify(List<?> lst) {
        return "List";
    }

    public static String classify(Collection<?> c) {
        return "Unknown Collection";
    }

    public static void main(String[] args) {
        Collection<?>[] collections = { 
            new HashSet<String>(), new ArrayList<BigInteger>(), 
            new HashMap<String, String>().values() };

        for( Collection<?> c : collections )
            System.out.println(classify(c));
    }
}
```
- 결과는 세번 다 Unknown Collection을 출력
	- Classify 메소드가 오버로딩되어 호출될 메소드가 컴파일 시점에 결정
> 이 결과에서 볼 때, 주어진 매겨변수에 대하여 Overloading된 메소드 중 어느것이 호출될 지 API의 사용자가 모른다면 사용 시 에러가 날 가능성이 높다

## 문제회피를 위해서
- 같은 수의 매개변수를 갖는 두 개의 Overloading 메소드를 절대 사용하지 않는것
- 메소드에서 가변인자를 사용한다면 그 메소드는 Overloading 하지 않는 것

## 생성자 Overloading
- 생성자의 경우 다른이름을 사용할 수 없다
	- 여러개의 생성자가 Overloading 될 수 있다
> 가능하다면 static 팩토리 메소드를 제공하는 것이 좋다

- 매개변수 타입의 캐스팅으로 인해 똑같은 집합의 매개변수들이 서로 다른 오버로딩 메소드로 전달될 수 있는 상황을 피해야 함