---
layout: post
title: Declarations and Expressions
description: "Declarations and Expressions"
tags: [Javascript]
---
### 선언(declaration)
- 미리 JS의 실행컨텍스트(execution context)에 로딩 되어 있으므로 언제든지 호출가능
- 함수는 Vo(variable object)에 저장됨.
- 사용은 쉬우나 많은코드가 vo에 저장되면 응답속도저하
- 스크립트파일을 모듈화하고 필요한 시점에 비동기방식으로 로딩하는것이 속도증가에 도움

### 표현식(Expression)
- 인터프리터가 해당 라인에 도달하였을때만 실행이 된다.
- Vo에 저장하지않고 runtime시에 해석&실행

### IIFE(즉시호출함수표현식)
- 괄호 쌍이 익명의 함수를 감싸서 함수 선언을 함수 표현식으로 표현될 수 있다.
- Ex) `(x = function () {});`
- 익명의 함수를 global scope에 선언하지 않고 어디서든 익명의 함수표현식을 가질 수 있다.
	
```javascript
(showName = function (name) {
    console.log(name || "No Name")
    }
) (); // No Name
showName("Rich"); // Rich
showName(); // No Name
```

- 이름이 부여되지 않은 익명의 표현식은 나중에 호출불가
- 익명함수가 괄호안에 위치될 경우, 전체그룹을 평가(evaluate)하고, 값을 리턴  
:리턴값은 익명함수 자신.
	
### Global scope를 오염시키지 않기 위해 사용.
- 전역변수의 선언을 피함.