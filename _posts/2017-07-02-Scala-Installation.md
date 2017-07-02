---
layout: post
title: Scala 설치하기 (feat.Ubuntu)
description: "Scala install to ubuntu"
tags: [Scala, Installation]
---
# 개요
- Functional programming 연습을 위해 Scala를 설치함
- 하스켈하려다가 말았음

# 과정
1. jdk 설치
- sudo apt-add-repository ppa:webupd8team/java
- sudo apt-get update
- sudo apt-get install oracle-java9-installer

2. Scala 설치
- http://www.scala-lang.org/download/ 에서 압축 다운
- sudo tar xvf 압축파일명 -C /usr/local/src/scala

3. 환경변수
- .bashrc
- export SCALA_HOME=/usr/local/src/scala/scala-버전
- export PATH=$SCALA_HOME/bin:$PATH
