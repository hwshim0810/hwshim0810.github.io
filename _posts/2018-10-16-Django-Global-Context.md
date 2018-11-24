---
layout: post
title: Django Template 에 전역 Context 추가하기
description: 'Add global context to django template'
tags: [Django]
---

## 개요

템플릿으로 페이지를 작성하다보면 많은 곳에서 필요한 Context 가 생기기도 하는데, 일일히 View 에서 추가해주는 것 보다 간단하게 처리할 수 있는 방법을 사용해 보았다.

---

### 예제:: 현재 사이트정보 전달하기

- 적당한 위치에 Context processor 를 만든다.

  - 전달할 Context 의 정의

```python
# example/context_processors.py

from django.contrib.sites.shortcuts import get_current_site

def site(request):
    return {
        'site': get_current_site()
    }
```

- Settings 에서 만들어 놓은 프로세서를 추가함

```python
TEMPLATES = [
    {
        # ...
        'OPTIONS': {
            'context_processors': [
                # ...
                'example.context_processors.site'
            ]
        }
    }
]
```
