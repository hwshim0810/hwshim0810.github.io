---
layout: post
title: Django Framework의 요청/응답 흐름
description: "Django request-response cycle"
tags: [Django, Python, Web]
---

## 개요
- 기본적인 Django component 에 대하여 알고 있다고 가정
- Django 어플리케이션이 요청을 받았을 때 응답을 내놓기까지의 과정을 간단히 요약

<figure>
	<a href="/images/django-cycle.jpg"><img src="/images/django-cycle.jpg" alt="Django request-response cycle"></a>
</figure>

## WSGI
- `웹 서버(Nginx, apache, ...)`와 `웹 프레임워크(Django, ROR, ...)`의 연결을 위한 인터페이스 도구
 - 둘 사이의 게이트키퍼 역할
- WSGI 의 응답을 처리하기 위해 서버는 웹 어플리케이션 `(여기서는 Django project::이하 앱)` 를 실행하고 콜백 기능을 제공
- 앱은 요청을 처리하고 제공된 콜백을 이용해 응답을 반환

## Middleware
- 웹 서버와 앱 사이에 존재
- 일종의 양방향 필터
- 둘 사이를 오가는 데이터를 가공할 수 있다.
- Django 의 Middleware 는 `process_request`, `process_response`, `process_view`, `process_exception` 중 하나 이상을 가지고 있어야 함
  - WSGI Handler 에 의해 수집되어 순서대로 호출될 Method 들

| Method 명 | 역할 |
| --- | --- |
| *process_request* |  데이터 흐름의 *요청* 단계에서만 사용됨. 반환값에 따라 `process_response`로 바로 넘어가기도 함. `None` 리턴의 경우 계속 진행 |
| *process_response* | 데이터 흐름의 *응답* 단계에서만 사용됨 |
| *process_view* | 요청 처리단계에서 사용됨. `process_response` 와 같이 반환값에 따라 진행경로가 바뀔수 있음.  |
| *process_exception* | `View`에서 처리중 예외가 발생한다면 예외단계에 속한 Method 리스트를 처리함. |


## 흐름
1. 요청이 들어오면 WSGI 처리기가 인스턴스화 된다.
2. 지정한 `settings.py` 파일과 *Django exception* 클래스들을 불러온다
3. `settings.py` 에서 `MIDDLEWARE_CLASSES` 또는 `MIDDLEWARES` 튜플을 통해 *미들웨어* 클래스를 불러온다.
4. 뷰, 응답, 요청, 예외를 처리할 *Middleware method list*가 작성된다.
5. *Request method* 들을 돌면서 처리함 :: `process_request`
 - 처리가 되면 `view` 에 전달될 `request` 객체가 준비됨.
6. *URL conf* 를 통한 URL 처리
 - URL 라우팅을 통해 호출할 `view` 를 결정
7. 뷰 처리 및 함수호출 :: `process_view`
 - `view` 는 세가지 항목이 포함되어야 한다
  - 호출가능해야 함 :: 함수/클래스 기반의 View (클래스기반이라면 `View 클래스 상속`& `as_view()`가 *Http Method* 기반)
  - `process_request`/`process_view` 에 의한 결과로 `HttpRequest` 객체를 첫번째 인자로 받아야 함
  - `HttpResponse` 객체를 반환하거나 예외를 발생시켜야 함 여기서의 응답이 `process_view` 에서 사용
 - 처리할 `View` 가 결정되면 `process_view` 단계의 미들웨어가 개입
8. 예외 메서드 처리 :: `process_exception`
9. 응답 메서드 처리 :: `process_response` (요청 미들웨어 처리의 역순으로)
 - 캐싱 등
 - 1.10 이하 버전에서는 `MIDDLEWARE_CLASSES`의 모든 미들웨어들이 `process_response`를 가지고 있고 호출함. (진행상황에 따라 `process_response`로 바로 이동한 경우에도!)
 - 이후 버전(`MIDDLEWARES`) 에서는 실행된 미들웨어와 이전에 실행된 미들웨어만이 `process_response`를 호출함
10. 응답 값을 만들에 웹서버 콜백으로 전달

#### 참조&번역: [Django Middlewares and the Request/Response Cycle](https://medium.com/zeitcode/django-middlewares-and-the-request-response-cycle-fcbf8efb903f)