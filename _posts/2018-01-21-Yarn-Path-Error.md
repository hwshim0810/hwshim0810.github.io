---
layout: post
title: Yarn global package 실행시 못찾는 문제
description: "Yarn global path error"
tags: [ErrorHandling, Yarn, Nodejs]
---
# 상황
- Yarn 설치 후, global add ~ 로 설치는 되나 찾지 못함 (Can not find...)

## 해결
```bash
# 경로확인
# 프로젝트 폴더로 경로가 잡혀있음
$ yarn bin
/home/.../node_modules/.bin

# 경로 추가
$ export PATH="$PATH:$(yarn global bin)"
```