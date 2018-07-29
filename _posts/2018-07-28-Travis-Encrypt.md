---
layout: post
title: Travis를 이용한 Android Build 과정의 암호화
description: "Travis file encrypt with android build"
tags: [Android, CI, Travis]
---

### Missing Google App Id :: google-services.json
- Firebase 서비스를 앱에 추가할 때, API Key등이 들어있는 `google-services.json`파일을 필요로 함
  - Secret 파일이기 때문에 `.gitignore`에 등록되어 있어 저장소에 올라가지 않음
  - 근데 빌드시에 저 파일과 Key를 찾음.

- 여러가지 시도 끝에 찾은 방법은 `암호화`
  - [Travis-File encrypt DOC](https://docs.travis-ci.com/user/encrypting-files)

- 중요 파일을 암호화 하여 저장소에 업로드 후 Build 시에 풀어서 사용

#### Travis CI CLI를 이용한 파일 암호화
- 필요조건
  - `ruby`

#### Travis CLI 설치 및 로그인

```bash
$ gem install travis
$ travis login  # or travis login --pro
```

#### 암호화

```bash
$ travis encrypt-file [암호화 할 파일::Target]
encrypting [Target file] for [저장소명]
storing result as [Target file].enc
storing secure env variables for decryption

Please add the following to your build script (before_install stage in your .travis.yml, for instance):
[암호화 결과]

```

#### 출력된 암호화 결과 빌드 스크립트에 추가

```yml
before_install:
    openssl aes-256-cbc -K $encrypted_[Hash값]_key -iv $encrypted_[Hash값]_iv -in [Target file].enc -out [Target file] -d
```
