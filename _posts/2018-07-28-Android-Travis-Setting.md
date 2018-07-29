---
layout: post
title: Travis를 이용한 Android Build 과정의 문제해결
description: "Android build error with travis"
tags: [Android, CI, Travis]
---

### Android & Travis
- [Travis-Android DOC](https://docs.travis-ci.com/user/languages/android/)
- 대체로 기본설정을 사용하면 돌아가는 듯 하나 빌드과정에서 생긴 문제

### 실행환경
- 앱의 Complie 버전은 27, 최소버전 23
- Travis 에서는 27과 24버전을 사용하기로 함.

#### `Invalid --abi armeabi-v7a for the selected target.`
- `echo no | android create avd --force -n test -t android-${API_VERSION} --abi armeabi-v7a` 를 통한 에뮬레이터 실행과정에서 발생함

#### 예제 (공식 DOC)

```yml
before_script:
  - echo no | android create avd --force -n test -t android-22 --abi armeabi-v7a
  - emulator -avd test -no-audio -no-window &
  - android-wait-for-emulator
  - adb shell input keyevent 82 &
```

- `-no-audio` 옵션은 시작하려는 에뮬레이터 버전 22까지만 유효
    - 23이상부터는 옵션을 제거해야 됐음.
- sdk 업데이트 구문을 삽입함
- Target 버전 못찾는 문제를 해결하기 위해 before_install 블록추가

#### 수정버전

```yml
before_install:
  - yes | sdkmanager "platforms;android-${COMPILE_SDK_VERION}"

before_script:
  - echo "y" | android update sdk -a --no-ui --filter android-${EMULATOR_SDK_VERSION}
  - echo "y" | android update sdk -a --no-ui --filter sys-img-armeabi-v7a-android-${EMULATOR_SDK_VERSION}
  - echo no | android create avd --force -n test -t android-${EMULATOR_SDK_VERSION} --abi armeabi-v7a
  - emulator -avd test -no-window &
  - android-wait-for-emulator
  - adb shell input keyevent 82 &
```
