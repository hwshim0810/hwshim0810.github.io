---
layout: post
title: Python/Django 임시 파일과 함께하는 API 테스트
description: "Upload file test with python-django"
tags: [Django, Python, Test]
---

## 이미지 파일 업로드 테스트하기

이미지 파일 필드가 있는 모델의 경우, 테스트용 파일을 따로 업로드 해놓는 것이 애매할 수 있다. 그것을 해결하기 위한 임시파일 생성과 테스트 후 생기는 파일의 처리법을 알아보자.

> 테스트는 Django Restframework `rest_framework.test.APITestCase` 를 이용함.

### 테스트 클래스

```python
from django.urls import reverse

from rest_framework.test import APITestCase

class ExampleTests(APITestCase):

    def test_create_model(self):
        url = reverse('example_create')
        ...

```

### 1. 파일 생성하기

- 존재하는 파일을 사용하는 경우
  - 실제 파일이 경로에 존재해야 함

```python
with open('/...파일경로', 'rb') as file:
    data = {
        'title': 'Title',
        'image_field': file
    }
    response = self.client.post(url, data, format='multipart')
```

- 임시파일 사용하기
  - 임의의 이미지 파일을 생성하여 업로드에 사용
  - 단순한 바이트 데이터를 이용하여 파일을 생성하게 될 경우 ImageField Validation 에 실패하게 된다.

```python
def get_test_image():
    file = BytesIO()
    image = Image.new('RGBA', size=(25, 25), color=(155, 0, 0))
    image.save(file, 'png')
    file.name = 'test_image.png'
    file.seek(0)
    return file

...

data = {
    'title': 'Title',
    'image_field': get_test_image()
}
response = self.client.post(url, data, format='multipart')
```

### 2. 테스트로 생성된 파일 처리하기

업로드 테스트의 결과로 파일은 Django 셋팅에서 지정한 MEDIA 경로로 업로드 된다. 테스트용 경로를 따로 지정해줄 필요가 있다. 방법은 두가지가 있는데,

- 테스트용 셋팅을 이용한 경로수정
- `@override_settings` 데코레이터를 사용한 경로수정  
  여기서는 데코레이터를 사용하여 업로드 경로를 수정한다.

```python
import tempfile

from django.test import override_settings


@override_settings(MEDIA_ROOT=tempfile.gettempdir())
def test_create_model(self):
    ...

# 혹은

@override_settings(MEDIA_ROOT=tempfile.TemporaryDirectory(prefix='media_test').name)
def test_create_model(self):
    ...
```

- `@override_settings` 데코레이터는 사용하고 있는 Django settings 의 값을 Runtime 에 대체해서 사용할 수 있게 한다.
- `tempfile.gettempdir()` 는 사용중인 OS 에 따라 `/tmp`, /`usr` 등의 임시 디렉토리 명을 반환한다
  - Ubuntu 환경에서는 `/tmp`
- `tempfile.TemporaryDirectory(prefix='media_test').name` 은 OS 임시디렉토리 아래에 prefix + 임의의 문자로 구성된 디렉토리명을 반환한다

### 임시 파일과 디렉토리

- Ubuntu System 은 기본적으로 임시 디렉토리의 파일을 재부팅시 초기 스크립트에 의해 삭제한다
  - `/etc/default/rcS` 의 `TMPTIME` 의 값 변경을 통해 수명을 조절할 수 있으며 기본은 `0`으로 매 재부팅마다 삭제
    - `-1`로 설정하면 삭제하지 않도록 함, `/var/tmp` 에 파일을 넣어도 삭제를 방지할 수 있다.
  - `tmpreaper` 패키지를 사용하면 오래 엑세스 되지 않는 파일을 삭제할 수 있다. (재부팅이 적은 서버에 적용)

### 참조문서

- [Django-doc::testing-tools](https://docs.djangoproject.com/en/2.0/topics/testing/tools/)
- [Restframework-doc::testing](https://www.django-rest-framework.org/api-guide/testing/)
