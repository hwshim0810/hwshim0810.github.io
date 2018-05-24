---
layout: post
title: Django 에서 fetch 를 이용한 요청
description: "Ajax request in Django to use fetch"
tags: [Django, Javascript, Web]
---

## 개요
- Django 에서 Ajax 요청을 보낼 때 일어난 일

## Example
- 일단 Example 부터

```javascript
var headers = new Headers();
var csrftoken = getCookie('csrftoken');
headers.append('X-CSRFToken', csrftoken);
headers.append('Accept', 'application/json, text/plain, */*');
headers.append('Content-Type', 'application/x-www-form-urlencoded');

fetch('/api',
      {
        method: 'post',
        headers: headers,
        credentials: 'include',
        body: JSON.stringify({
            is_man: true,
            numbers: [1,2,3],
            content: 'Content!!'
        })
      }
)
.then(_ => {
  // ...
}).catch(
  err => console.log(err)
);
```

- Django 에서는 데이터 전송시 정상적 페이지인지 확인을 위해 CSRF 토큰을 사용
- 페이지와 쿠키(혹은 세션)에 던저준 CSRF 토큰을 이용해야 함.
- 토큰은 헤더에 셋팅

- 쿠키꺼내는 Method는 [Gist](https://gist.github.com/hwshim0810/33f5771df8769ea4a9a907c0c1237cf3)
- 자세한 정보는 [Doc](https://docs.djangoproject.com/en/1.11/ref/csrf/)