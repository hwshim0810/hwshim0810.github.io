---
layout: post
title: Javascirpt Variable
description: "JS Variable"
tags: [Javascript]
modified: 2017-05-06
---
## 변수 호이스팅(Variable Hoisting)

- 모든 변수선언은 호이스트 된다.
- 변수의 정의가 그 범위에 따라 선언과 할당으로 분리
- 변수가 함수 내부라면 선언이 함수의 최상위
- 함수 바깥에서 정의되었다면 전역컨텍스트 최상위	
- 변수의 선언이 초기화나 할당시에 발생하는 것이 아니라, 최상위로 호이스트된다.

```javascript
function showName() {
// var name; // 내부에서 해석하였을 때 최상위에서 선언한것으로 간주
console.log("First Name : " + name);
var name = "Ford";
console.log("Last Name : " + name);
}
showName();
// First Name : undefined
// Last Name : Ford
// First Name이 undefined인 이유는 지역변수 name이 호이스트 되었기 때문입니다.
```

호이스트 되었을때, 함수 선언은 변수선언을 덮어 씁니다.

```javascript
// 다음 두 변수와 함수는 myName으로 이름이 같습니다.
var myName; // string
function myName() {
console.log("Rich");
}
// 함수 선언은 변수명을 덮어 씁니다.
console.log(typeof myName); // function
```

하지만, 변수에 값이 할당될 경우에는 반대로 변수가 함수선언을 덮어 씁니다.

```javascript
var myName = "Richard";
function myName() {
console.log("Rich");
}
console.log(typeof myName); //string
```

## Let, const (ES6)

- let으로 선언된 변수는 전역범위가 아닌 블록범위로 인정
> 블록 밖에서는 var와 같음
- const는 상수처럼 작동(Immutable)