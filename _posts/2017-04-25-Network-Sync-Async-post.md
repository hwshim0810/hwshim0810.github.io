---
layout: post
title: Sync/Async/Blocking/Non-Blocking
description: "Sync/Async/Blocking/Non-Blocking"
tags: [Network, OS]
---
# Base
- I/O작업은 User-Level에서 직접 수행할 수 없다
- 실제 I/O작업은 Kernel-Level에서만 가능
> 따라서 User-Process(or Thread)는 Kernel에게 I/O를 요청해야 함

# Synchronous(동기)
- 작업을 요청한 후 해당 작업의 결과가 나올 때까지 기다린 후 처리하는 것
- Blocking과 같은 개념
> 예) 주문을 하고 나올때까지 기다렸다가 받아감

# Asynchronous(비동기)
- 작업을 요청해놓고 딴 일을 하다가 해당작업이 완료되면 그 때 완료되었음을 통지받고 그에 따른 처리를 하는 것
- 운영체제의 비동기 API를 통해 이루어지며 IO작업이 완료되면 그에 적합한 Handler를 통하여 처리를 한다
> 예) 주문을 하고 간다음 나오면 전화를 해서 받아가게 함

# Blocking
- IO작업에서 Blocking으로 동작할 경우 해당 IO가 끝날때까지 대기해야함  
 Application 실행 시, 운영체제 대기 Queue에 들어가면서 요청에 대한 System call이 완료된 후에 응답을 보내는 경우에 해당

# Non-Blocking
- Non-Blocking일 경우, 작업을 완료할 수 있다면 완료  
 Application 실행 시, 운영체제 대기 Queue에 들아가지 않고 실행여부와 관계없이 바로 응답을 보낼 경우에 해당

## Non-Blocking vs Asynchronous
- Non-Blocking
	- System call return 시 실행된 결과와 함께 반환된 경우 
	- 요청에 바로 응답할 수 있는 경우는 응답, 바로 응답하기 힘든 경우 에러 return
	- 에러를 받을 경우, 데이터를 정상적으로 받을 때 까지 재요청
- Asynchronous
	- System call return 시 실행된 결과와 함께 반환되지 않는 경우
	- 요청의 처리 완료여부와 관계없이 바로 응답
	- 이후 운영체제에서 응답할 준비가 완료되는 시점(데이터를 받는 요청이라면 데이터가 준비되었을 때)에 응답한다

## Synchronous vs Asynchronous
- Synchronous
	- System call의 완료를 기다림
- Asynchronous
	- System call의 완료를 기다리지 않음

## Synchronous vs Blocking
- Synchronous
	- 시스템의 반환을 기다리는 동안 대기 큐에 머무는 것이 필수가 아님
- Blocking
    - 대기 큐에 머무는 것이 필수

## 정리
<figure>
	<a href="/images/sync.jpg"><img src="/images/sync.jpg" alt="Sync/Async..."></a>
</figure>