---
layout: post
title: Websocket 이 뭐지?
description: "About websocket"
tags: [Web, Network]
---
# 개요
- 서버에서 클라이언트를 호출할 수 있음
> 웹 소켓은 TCP소켓의 두가지 방법으로 커뮤니케이션하는 일종의 푸시(PUSH)기술

## HTTP의 문제
> 기본적으로 연결을 맺고 요청을 보낸 후 응답을 받고 연결을 끊는 방식  
기존의 HTTP는 Client-Server간 접속유지를 하지 않는 Half-duplex(반이중) 통신

- 재요청, Ajax등은 polling 방식 (Client가 Server에 데이터를 요청)
	- 실시간 데이터의 업데이트 주기 예측이 어려워 불필요한 요청이 발생함
	- 모든 요청에 헤더파일이 중복되어 낭비가 생김

## 보완을 위한 노력
### Comet (Long-Polling)
- 서버는 받은 요청을 일정시간 동안 대기
- 데이터가 업데이트 되었다면 응답을 돌려주고 다시 요청을 받음
- 대기시간이 지났다면 응답을 끊고 재연결
> 업데이트가 빈번한 경우 Polling에 비해 이점이 크지않다

***
# 웹 소켓의 등장
- HTML5에서 등장한 websocket protocol
- 사용형태 : `ws://~~`
- HTTP기반이지만 전혀 다른 프로토콜
	- 웹 소켓 연결에 http가 개입함
	- Handshake과정이 성공적으로 끝난 경우
		- http를 웹 소켓 프로토콜로 변경(Protocol switching)

# 이점
- 최초접속을 제외하고는 헤더정보를 보내지 않음(네트웍 비용이득)
- 양방향 통신(Full-duplex)
	- 대기 없는 빠른 구현
- 데이터의 구조는 텍스트와 바이너리 모두 양방향 가능
- 텍스트는 시작 바이트가 0x00 끝이 0xFF : UTF-8 데이터 포함가능

# 연결 예시

<figure>
	<a href="/images/websocket.png"><img src="/images/websocket.png" alt="Websocket connection"></a>
</figure>
