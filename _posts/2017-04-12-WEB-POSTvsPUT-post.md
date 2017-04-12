---
layout: post
title: POST vs PUT
description: "POST vs PUT"
tags: [Web, REST API, HTTP Method]
---
## 필요개념
### Idempotent : 멱등의

- 몇 번이고 같은 연산을 반복해도 같은 값이 나온다는 것
> 예시) f(x) = f(f(x))
	
- Fault-tolerant API 디자인에 있어 중요한 요소
> 예시) POST : /users 요청 시 어떤 이유로 요청이 time-out(408) 되었다고 가정하면  
클라이언트는 요청이 전달되었으나 네트워크가 끊어졌는지 요청 자체가 전달이 되지 않았는지 확인이 불가능
	
- 클라이언트가 원하는 operation이 idempotent하다면 재 요청해도 상관이없다
	- 항상 같은 결과를 만들기 때문
	- **`POST` 는 idempotent하지 않다**
	
## POST
- *클라이언트*가 **리소스의 위치를 지정하지 않았을 때** 리소스를 생성하기 위함

```
POST /dogs HTTP/1.1 
{ "name": "blue", "age": 5 } 
HTTP/1.1 201 Created
```
- 이 경우 `/dogs/2`에 생성, `/dogs/3` 처럼 매번 다른 곳에 리소스 생성이 우려됨
- *idempotent 하지 않음*
- POST to a URL creates a child resouce at a server defiend URL(RFC 2616 POST)

## PUT
- **리소스의 위치가 지정되었을 때** *생성* 또는 *수정(업데이트)*를 위해 사용

```
PUT /dogs/3 HTTP/1.1
{ "name": "blue", "age": 5 }
```
- 이 경우 `/dogs` 의 속성이 name, age뿐이라면 몇번을 수행하더라도 같은 결과 보장
- **idempotent** 함
- PUT to a URL create/replaces the resource in is entirely at the client defined URL  
(RFC 2616 PUT)
	
## PATCH
- `PUT`은 리소스의 모든 속성을 업데이트 하기 위해 사용
- `PATCH`는 부분만을 업데이트하기 위해 사용
	- `PUT`과 마찬가지로 리소스의 위치를 클라이언트가 알고 있을 경우
	- PATCH to a URL updates part of the resource at that client defined URL  
	(RFC 5789: Patch Method for HTTP)
	
## Response Code
- `POST`나 `PUT` 요청이 리소스를 새로 생성할 경우
	- 리소스의 위치를 response header의 Location field에 담아 201(Created)를 보낼 수 있다
- not-identifiable한 리소스를 생성 할 경우
	- 200(OK) 또는 204(No Content)를 보낼 수도있다
- Async하게 서버가 처리한다면, 요청은 수락되었으나 아직 커밋되지 않았음을 알려야 함
	- 202(Accepted)를 보냄
	
## Safe Methods
- 리소스를 수정하지 않는 Method들을 safe하다고 한다
	○ OPTIONS, GET, HEAD 등등
> **OPTIONS** : 현재 웹서버에서 지원하는 Method에 대한 정보  
> **HEAD** : GET요청에서 Body는 제외하고 Header에 대한 정보
즉, **해당 리소스가 존재하는지 / 문제가없이 제대로 처리가 되는지** 에 대한 정보
	
- 대부분의 경우 idempotent하면 safe하다고 한다
    - 예외) DELETE : idempotent하지만 리소스를 변경하므로 safe하지 않다
	
## Cacheable Methods
- OPTIONS Method에 대한 응답은 캐시가 불가능
- 리소스는 URI에 대한 정보인데 OPTIONS은 정보를 가져오는 것이 아니라,  
정보에 대해 어떤 연산이 가능한지를 알려줌
- HTTP에서는 정보에 대해 캐싱
	- 예시) GET, HEAD…

## Trace, Connect
- **TRACE**는 클라이언트가 방금 보낸 요청을 다시 달라고 서버에게 요청하는 것
- **CONNECT**는 HTTP 터널링을 할 때 쓰임

- 중간의 프록시 서버를 위해서는 CONNECT로 요청, 마지막 프록시에서 end-point로는  
GET또는 CONNECT를 보냄. HTTPS라면 CONNECT, HTTP라면 아무거나
