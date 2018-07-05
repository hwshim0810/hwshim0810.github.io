---
layout: post
title: Django Custom Command 활용하여 더미 데이터 만들기
description: "Making dummy data use for djago custom command"
tags: [Django, Python]
---

## 개요
- Django 의 Custom 명령어를 이용하여 테스트용 더미 데이터를 생성해보았다.

### 명령어 만드는 법 :: 기초
- [Django Custom Command 만들기](https://laziness.xyz/2018/07/Django-Custom-Command)

### 필요한 패키지
- Django framework
- 더미 데이터 모델정의를 위한 `factory-boy`
- 더미 데이터 생성을 위한 `Faker`

### 대상 모델
```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=20)
    profile_image_path = models.CharField(max_length=150)
    birth_at = models.DateTimeField(auto_now_add=True)
    died_at = models.DateTimeField(null=True)

class Job(models.Model):
    name = models.CharField(max_length=20)
    person = models.ForeginKey(Person)

```

### 더미 데이터 생성 모델 정의
- 생성가능한 더미 데이터의 정보는 [Faker Docs](https://faker.readthedocs.io/en/latest/index.html)참조
```python
# _provider.py
import factory
from faker import Factory

fake_ko = Factory.create('ko_KR')  # 한글 더미 데이터 생성기
fake_en = Factory.create()  # 영어 더미 데이터 생성기

class PersonFactory(factory.django.DjangoModelFactory):
    
    class Meta:
        model = 'app.Person'

    name = factory.sequence(lambda _ : fake_ko.name())  # 람다 변수는 번호로 나오니 데이터에 필요하다면 이용가능
    profile_image_path = factory.sequence(lambda _ : fake_ko.image_url())
    died_at = factory.sequence(lambda _ : fake_ko.future_date(end_date='+30d'))


class JobFactory(factory.django.DjangoModelFactory):

    class Meta:
        model = 'app.Job'

    name = factory.sequence(lambda _ : fake_ko.job())
    person = factory.SubFactory(PersonFactory)
```

### 명령어 정의
```python
from django.core.management.base import BaseCommand, CommandError

from . import _provider


class Command(BaseCommand):

    help = '더미 데이터 생성기'

    def add_arguments(self, parser):
        parser.add_argument('cnt', nargs=1, type=int)

    def handle(self, *args, **options):
        try:
            cnt = int(options['cnt'][0])
        except ValueError:
            raise CommandError('Not integer!')

        if cnt <= 0:
            raise CommandError('greater than or equal to 1.')

        for i in range(cnt):
            _provider.PersonFactory()  # Person 모델만 생성
            _provider.JobFactory()  # Person 모델생성 & 연결된 Job 모델생성

```

### 사용 예
```bash
$ python manage.py [1 ~ ]
```