---
layout: post
title: Django Distinct field 에러
description: "Django ORM distinct field error"
tags: [Django, Python]
---

### 개요
- ORM 을 이용해 조회하던 중 `DISTINCT ON fields is not supported by this database backend` 에러가 발생

```python
# 시도
ExampleModel.objects.filter(...).distinct('some_column').count()
```

- PostGresql 에서는 별 문제없음
- MySQL-Django 조합에서는 지원이 안됨

### 해결
```python
ExampleModel.objects.filter(...).values('some_column').distinct().count()
```