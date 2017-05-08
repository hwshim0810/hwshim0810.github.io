---
layout: post
title: With statement in python
description: "With statement in python"
tags: [Python]
---
# 개요
- Python 2.5에서 도입
> 어떤 Block에 진입하고 나올 때 **지정된 객체(Context manager)**로 하여금 그 시작과 끝에서 어떤 처리를 하도록 할 때 사용
	
# 예제

```python
 # 파일작업, I/O, 리소스 획득 등등...
 some things up logic... 
 
 try:
  do something...
 finally:
  # 닫기, 해제 등 무조건 실행되야 할 작업
  tear things down 
 
 # with문으로 교체
 class some_do:
  def __enter__(self):
   set things up...
   return thing
 
  def __exit__(self, type, value, traceback):
   tear things down

 with some_do() as thing:
   something do ... using thing
```
- A는 with Block이 시작되는 시점에 A.\__enter__()를 호출
- 끝나는 지점에 A.\__exit__()를 호출
> Block내부에서 Exception이 발생하더라도 A.\__exit__()의 호출이 보장된다

- 분리를 통한 **재사용성 증가**

# file로 구현되어 있는 예제

```
>>> f = open("x.txt")
>>> f
<open file 'x.txt', mode 'r' at 0x00AE82F0>

>>> f.__enter__() // file obj 자신을 return
<open file 'x.txt', mode 'r' at 0x00AE82F0>

>>> f.read(1)
'X'

>>> f.__exit__(None, None, None) // close
>>> f.read(1)

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file
```

- With를 통한 사용 예
	- 열고, 사용한 후 닫는것을 보장
    
```python
with open("x.txt") as f:
    data = f.read()
    do something
```
