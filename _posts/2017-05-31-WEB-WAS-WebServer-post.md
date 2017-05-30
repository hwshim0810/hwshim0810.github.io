---
layout: post
title: WSGI와 WAS
description: "WebServer/WSGI/Middleware"
tags: [Web, Python]
---
## 부제 : Web server, WSGI, Middleware, WAS 의 개념

# Web Server
- 정적이다
- 요청이 들어오면 요청을 분석하여 알맞은 리소스를 리턴
- 동적으로 만들기 위하여 웹서버위에 django, node.js 등의 프레임워크를 올림
    - WAS가 됨

# WAS
- 웹 서버위에 서버 어플리케이션을 올린 것

# WSGI
> Web Server Gateway Interface
	
- Python에서 Application, 즉 Python Script가 웹 서버와 통신하기 위한 명세
    - 프로토콜의 개념
- 서버와 앱 양단으로 나뉘어져 있다
    - WSGI 요청을 처리하려면 서버에서 환경정보와 콜백함수를 앱에 제공해야 한다.
    - 앱은 그 요청을 처리하고 콜백함수를 통해 서버에 응답한다

# Middleware
- 서버의 관점에서는 앱
- 앱의 관점에서는 서버로 행동
- Tornado, mod_wsgi, uwsgi, gunicorn 등이 있다
- Middleware를 통해 앱이 실행되므로 앱을 실행시켜주는 어플리케이션 컨테이너 라고도 할 수 있다.
	- Java의 Servlet 컨테이너와 동일한 개념

## 흐름
- Http 요청이 들어온다
- 웹 서버가 요청을 받는다
	- 서버사이드 처리가 필요없으면 응답리턴(static한 웹 서버)
- 서버사이드 처리가 필요하면 WSGI를 통해 Python Application으로 요청 전달
- Python Application이 요청을 받아 처리
- WSGI -> 웹서버를 통해 응답리턴

참고: <http://khanrc.tistory.com/entry/WSGI%EB%A1%9C-%EB%B3%B4%EB%8A%94-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%9D%98-%EA%B0%9C%EB%85%90> 