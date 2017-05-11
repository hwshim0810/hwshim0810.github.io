---
layout: post
title: location.href 와 replace
description: "Difference of location.href & replace"
tags: [Javascript]
---
# location.href
- 새로운 페이지로 이동
- 속성

```javascript
loaction.href = 'abc.html'
```

- 주소 기록이 됨
- 이전 페이지로 이동할 수 있다

# location.replace
- 현재 페이지를 새로운 페이지로 변경
- 메서드 형태로 사용

``` javascript
location.replace('acb.html')
```

- 주소 기록이 남지 않는다
- 이전 페이지로 이동이 불가함
    - 보안상 이전 페이지 이동 막기 위함