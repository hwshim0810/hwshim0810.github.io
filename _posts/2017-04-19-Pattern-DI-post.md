---
layout: post
title: 의존성 주입(DI)이란?
description: "About DI(Dependency Injection)"
tags: [DesignPattern, Spring]
---
# DI(Dependency Injection)
> 의존적 주입
 
 - 전략패턴(Strategy Pattern)을 일종의 프레임워크처럼 사용할 수 있도록 만들어짐
	- 객체 또는 구성요소간의 종속관계를 소스코드가 아닌 외부의 설정파일을 통하여 주입
	- 한 객체안에 다른 객체가 사용되면 두 객체간에 종속관계가 생성된다고 할 수있다

## Reference전달에 따른 DI

<figure>
	<a href="images/di00.png"><img src="images/di00.png" alt="DI diagram"></a>
</figure>
- A가 B에 의존하고 있음, B는 A에 의존하지 않는다
	- 즉, B는 A의 변화에 영향을 받지 않는다
	> 의존관계는 방향성이 있다

- B의 기능이 추가/변경되면 그 영향이 A로 전달된다

<figure>
	<a href="images/di01.png"><img src="images/di01.png" alt="Dependency Connect"></a>
</figure>
- ConnectioMaker 인터페이스가 변하면 UserDao가 영향을 받음  
인터페이스를 구현한 Class인 DConnectionMaker가 다른것으로 바뀌거나 그 내부에서 변화가 생겨도 UserDao에 영향을 주지않게 할 수 있다
> 인터페이스에 대해서만 의존관계를 만들어두면 변화에 영향을 덜 받는 낮은 결합도를 가지게 되면 그만큼 변경에 자유로워진다

## DI유형
a. 생성자를 이용(Constructor Injection)
- Has-A 관계
- 필요한 의존성을 모두 포함하는 클래스의 생성자를 만들고 그 생성자를 통해 의존성 주입
> 생성자에 파라미터를 만들고 이를 통해 DI컨테이너가 의존할 Object 참조를 넘겨줌

```java
class MovieLister...  
    public MovieLister(MovieFinder finder) {
        this.finder = finder;       
    }
class ColonMovieFinder...  
    public ColonMovieFinder(String filename) {
        this.filename = filename;
    }
```

b. setter() Method를 이용(Setter Injection)
- 의존성을 입력받는 setter()를 만들고 이를 통해 **의존성 주입**
> 외부에서 제공받은 Object 참조를 저장해뒀다가 내부의 Method에서 사용하게 하는 방식의 DI에서 적합하다

```java
// Spring Container
class MovieLister {...  
    private MovieFinder finder;
    public void setFinder(MovieFinder finder) {
        this.finder = finder;
    }
class ColonMovieFinder...  
    public void setFilename(String filename) {
        this.filename = filename;
    }
}
```

c.초기화 인터페이스를 이용(Interface Injection)
- 의존성을 주입하는 포함한 인터페이스를 작성하고 이 인터페이스를 구현하도록 함으로써 실행시에 이를 통하여 **의존성**을 주입
> Spring은 지원하지 않는 방식

```java
// Avalon
public interface InjectFinder {  
    void injectFinder(MovieFinder finder);
}

class MovieLister implements InjectFinder...  
    public void injectFinder(MovieFinder finder) {
        this.finder = finder;
    }
    
public interface InjectFinderFilename {  
    void injectFilename (String filename);
}

class ColonMovieFinder implements MovieFinder, InjectFinderFilename......  
    public void injectFilename(String filename) {
        this.filename = filename;
    }
```

참고 : http://choiwy.tistory.com/