---
layout: post
title: Django Custom Command 만들기
description: "Making custom command in django"
tags: [Django, Python]
---

## 개요
- Django framework 에서는 기본적으로 제공되는 `createsuperuser`, `makemigration`... 등의 명령어 이외에도 사용자가 직접 명령어를 만들 수 있는 기능이 있다.
- 공식 설명서에서는 Unix Crontab / Windows 스케쥴러 등의 작업에 활용을 권하고 있다.

### 참고
- [Django custom commands](https://docs.djangoproject.com/en/2.0/howto/custom-management-commands/)

### 과정
- 공식설명서에서 예시로 들어준 커스텀 명령어 모듈이 위치해야하는 곳
```
polls/
    __init__.py
    models.py
    management/
        __init__.py
        commands/
            __init__.py
            _private.py
            closepoll.py
    tests.py
    views.py
```
- Django 설정의 `INSTALLED_APPS` 에 등록된 APP의 패키지 :: (예시에서는 polls) 아래에 `management/commands` 를 만들었다.
  - Django는 앱 패키지 내부의 `management/commands`아래의 밑줄`_`로 시작하지 않는 모듈명을 명령어로 읽어 등록한다.
  - 위쪽예시에서 등록된 명령어는 `closepoll`

#### 모듈
- 명령어 모듈은 `django.core.management.base.BaseCommand`를 상속받는 `Command`라는 이름의 클래스를 정의해야한다.  

```python
from django.core.management.base import BaseCommand, CommandError
from polls.models import Question as Poll


class Command(BaseCommand):

    # python manage.py closepoll [-h | --help] 에 의해 출력될 문자열
    # 명령어에 대한 간단한 설명이 들어가면 된다.
    help = 'Closes the specified poll for voting'

    # 명령어에 받을 인자를 정의함
    def add_arguments(self, parser):
        parser.add_argument('poll_id', nargs='+', type=int)

    # 실제 처리부분
    def handle(self, *args, **options):
        for poll_id in options['poll_id']:
            try:
                poll = Poll.objects.get(pk=poll_id)
            except Poll.DoesNotExist:
                raise CommandError('Poll "%s" does not exist' % poll_id)

            poll.opened = False
            poll.save()

            self.stdout.write(self.style.SUCCESS('Successfully closed poll "%s"' % poll_id))
```
- 콘솔에 무엇인가를 출력하고 싶다면 `self.stdout.write()` 또는 `self.stderr.write()`를 사용하면 된다.
- `add_arguments` 의 인자중 `nargs`는 명령어로 받을 인자의 갯수를 정의한다.
  - 인자 파싱에 관한 Python 설명서:: [argparse](https://docs.python.org/3/library/argparse.html#module-argparse)
  - 여러개를 받고 싶다면 '+'
    - 여러개를 받았다면 `handle`의 `options`에 인자명을 키값으로 하여 값을 받음
  - 숫자를 입력하면 숫자만큼의 명령어 인자를 받게됨
  - ex) 위의 예시에서 1을 지정한 경우 `parser.add_argument('poll_id', nargs=1, type=int)`  
    
    
```bash
$ python manage.py closepoll 5  -> OK
$ python manage.py closepoll 5 22
...
error: unrecognized arguments: 22
```

- 명령어 처리에 대한 예외는 `CommandError`로 raise

- 이 외에도 `add_argument` 에 필수 명령어 인자외에도 옵션 인자를 추가할수도 있고, 기존의 명령어를 Override 하는 것도 가능하다.