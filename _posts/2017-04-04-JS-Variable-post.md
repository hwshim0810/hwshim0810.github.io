---
layout: post
title: Javascirpt Variable
description: "JS Variable"
tags: [Javascript]
---
## 변수 호이스팅(Variable Hoisting)

- 모든 변수선언은 호이스트 된다.
- 변수의 정의가 그 범위에 따라 선언과 할당으로 분리
- 변수가 함수 내부라면 선언이 함수의 최상위
- 함수 바깥에서 정의되었다면 전역컨텍스트 최상위	
- 변수의 선언이 초기화나 할당시에 발생하는 것이 아니라, 최상위로 호이스트된다.

## Let, const (ES6)

- let으로 선언된 변수는 전역범위가 아닌 블록범위로 인정
> 블록 밖에서는 var와 같음
- const는 상수처럼 작동(Immutable)