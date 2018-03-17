---
layout: post
title: Jekyll Plugin 적용기
description: "Apply plugin in jekyll"
tags: [Blog, Jekyll]
---

## 개요
- 글 내용에 Emoji를 넣고 싶어서 찾다보니 플러그인이 필요했음.
- 근데 찾다보니 플러그인이 다양한게 많아서 다른것들도 차근차근 적용해 볼 예정

### 제약사항
- Github-page 이용시에는 Github에서 이용가능한 것 이외에는 사용불가
  - Github에서 사용중인 것들 [링크](https://pages.github.com/versions/)
- 굳이 쓰고 싶다면 로컬에서 페이지 생성 후 _site의 결과물을 모두 올릴것
  - 추가 플러그인 이용하게 되는 경우 내용추가 예정

### Emoji 적용
- Gemfile에 `gem 'jemoji'` 추가
- _config.yml 에 plugins 에 `- jemoji` 추가
  - 현재 버전에 따라 옵션이 다를 수 있는데, bundle 있는 경우 `bundle update'로 맞춰주면 됨.
- Emoji는 [링크](https://github.com/buildkite/emojis) 참조
  - 모두 다 지원되지는 않는데, 새로운 Emoji 들은 따로 추가가 필요함.