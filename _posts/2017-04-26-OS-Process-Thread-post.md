---
layout: post
title: 프로세스와 스레드
description: "Process and Thread"
tags: [OS]
---
# 프로세스
> 운영체제로 부터 자원을 할당받는 **작업의 단위**

- 실행 시 운영체제로 부터 프로세서, 주소공간, 메모리 등 리소스를 할당 받음

## 구성
- 실행 스택(Stack)
	- 호출된 프로시저(함수)의 복귀주소와 인자값 저장
	- 지역변수 저장
- 실행 힙(Heap)
	- 텍스트(Code)영역과 별도로 유지되는 자유영역
	- 동적할당(malloc 등) : 프로그래머에 의한 할당
- 정적변수(Data)
	- 프로세스 실행 중 동적으로 할당받는 영역
	- 전역(Global) 또는 정적(Static)변수저장
- 텍스트(Code)
	- 프로세서가 실행하는 코드(명령어) 저장

## 종류
- 운영체제 프로세스
	- 커널프로세스 또는 시스템 프로세스
	- 프로세스 실행순서 제어, 사용하는 프로세스가 다른 사용자나 운영체제 영역을 침범하지 못하게 감시하는 기능
	- 사용자 프로세스 생성, 입출력 프로세스 등 시스템 운영에 필요한 작업수행

- 사용자 프로세스
	- 사용자 코드수행

# 스레드
> 프로세스가 할당 받은 자원을 이용하는 **실행의 단위**

- 한 프로세스 내부에서 동작되는 여러 실행의 흐름
- 프로세스 내의 주소공간이나 자원들을 같은 프로세스의 스레드끼리 공유
	- 각 스레드는 독립적 스택영역을 가짐
	- Code, Data, Heap 영역을 공유
	
## 장점
- 멀티 프로세스 작업을 멀티 스레드로 실행할 경우
	- 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리
- 프로세스의 통신보다 스레드간의 통신비용이 적다
	
## 단점
- 스레드간의 자원공유는 전역변수를 이용하므로 동기화 문제 주의필요
