---
layout: post
title: Diffrence of Querydict and dict
description: "Diffrence of Querydict and dict"
tags: [Django]
---

### 개요
- Django를 통한 작업 중, 요청 데이터를 사용하기 위해 request.data 로 반환받은 dict의 타입이 다르다는 것을 발견

### Querydict
- Django 로 요청을 보냈을 때, 요청에 대한 메타 데이터를 포함한 HttpRequest 객체를 생성함
- HttpRequest 객체에서 GET/POST 속성이 django.http.QueryDict 의 인스턴스
  - 기본적으로 MultiValueDict 라고 볼 수있다.
  - 그 이유는 HTML 의 폼은 동일한 키에 대하여 여러값이 있을 수 있기 때문
- 이 객체는 기본적으로 Immutable 이라 수정하기 위해서는 .copy() 를 사용해야 함.


### 주의점
- Restframework 사용시에 요청 media type 에 따라 Parser가 일반 dict를 반환하기도 하기 때문에, 타입에 주의해야 함.
  - 내장 dict 와는 다른 타입