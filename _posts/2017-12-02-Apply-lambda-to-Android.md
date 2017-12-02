---
layout: post
title: Android 에서 Java8 lambda 표현식 사용하기
description: "Apply to Java8 lambda"
tags: [Android, Java]
---
# 기준
- API 레벨 24 이상에서는 기본지원 : 옵션변경
- 이하버전 : retrolambda 이용


## API Level 24미만
### build.gradle
> *Project*

```gradle
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'me.tatarka:gradle-retrolambda:3.7.0'
    }
}
```

### build.gradle
> *Module : app*

```gradle
apply plugin: 'me.tatarka.retrolambda'
 
android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
---

## API Level 24 이상

### build.gradle
> *Module : app*

```gradle
android {
	...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
 }
```