---
layout: post
title: Kotlin takeIf/takeUnless 사용법
description: "Usage takeIf/takeUnless"
tags: [Kotlin]
---

## 개요
- 코틀린 기본함수 중 takeIf, takeUnless 의 사용법을 알아봄

## takeIf
`inline fun <T> T.takeIf(predicate: (T) → Boolean): T?`

1. 호출하는 주체는 Object T 자신
2. predicate function 의 파라미터로 호출주체(T)를 전달 
3. predicate 의 판단결과(Boolean) 에 따라서 null 혹은 자기자신(this)를 return

- 사용 예

```kotlin
// 1. Null check 와 Boolean 의 결합
if (obj != null && boolStatus)
  someFunc()

// 일반적 처리
obj?.takeIf{ boolStatus }?.apply{ someFunc() }

// 2. 호출 객체의 속성과 메서드를 사용하는 경우
if (obj != null && obj.boolStatus)
  obj.someFunc()

obj?.takeIf{ boolStatus }?.someFunc()
```

- Null 처리도 가능

```kotlin
// Null handling
obj?.takeIf{ boolStatus }?.apply{ someFunc() } ?: nullHandling()
```

## 주의점
```kotlin
// 예상한 결과 반환
obj?.takeIf{ boolStatus }?.apply{ someFunc() }

/** Danger
/* takeIf 가 null 을 리턴한 경우에도 실행이 되기 때문에
/* takeIf 가 False 인 경우에도 apply 블럭을 실행함
 */

obj?.takeIf{ boolStatus }.apply{ someFunc() }
```

## takeUnless
`inline fun <T> T.takeUnless(predicate: (T) → Boolean): T?`

- takeIf 의 반대버전
- takeUnless 가 True 인 경우 null 리턴, False 인 경우 호출자 리턴