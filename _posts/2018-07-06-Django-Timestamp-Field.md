---
layout: post
title: Restframework 에서의 Timestamp 변환을 위한 Customfield
description: "Making custom field in Restframework"
tags: [Django, Python, Restframework]
---

## 소개
- Date, DateTime 형식의 데이터를 Timestamp 로 변환할 때 사용가능한 CustomField

### Code
```python
from django.utils import timezone
from rest_framework import serializers


class DateToTimestampField(serializers.Field):
    def to_representation(self, value):
        epoch = timezone.datetime(1970, 1, 1)
        value = timezone.datetime.fromordinal(value.toordinal())
        return int((value - epoch).total_seconds())


class DatetimeToTimestampField(serializers.Field):
    def to_representation(self, value):
        epoch = timezone.datetime(1970, 1, 1)
        return int((value - epoch).total_seconds())
```