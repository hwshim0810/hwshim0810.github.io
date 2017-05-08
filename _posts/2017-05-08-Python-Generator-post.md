---
layout: post
title: 파이썬의 Generator&Yield
description: "With statement in python"
tags: [Python]
---
# 개요
> Iterator를 생성해주는 function

- Iterator는 **next()** method를 이용하여 데이터에 순차적으로 접근이 가능한 객체

# 일반 function과의 차이
- Yield 구문의 포함
- 일반 함수는 사용이 종료되면 결과를 호출한곳으로 **return**
- 그 후, 함수자체를 종료하고 메모리에서 **Clear**
	
# yield
- Generator함수는 yield를 만나면 해당 함수는 그 상태로 정지
- 반환값은 next()를 호출한 곳으로 전달
- 그 후, 함수가 종료되지 않고 그 상태로 유지
	- 함수에서 사용된 지역변수 등 함수 내부의 데이터가 **메모리에 유지**

## 예시

```python
 def gen(n):
  i = 0
  while i < n:
   yield i
   i += 1

 for x in gen(5):
  print(x)
```
1. 우선적으로 for block의 gen()이 호출됨
2. 함수 내부에서 진행하다 while 내부에서 yield를 만남
3. Return 을 만난것 처럼 호출부로 반환 (여기서는 0) 그 후 함수가 종료되지 않음
4. X에 yield로 받은 0이 대입되고 print 후 다시 반복하여 gen()을 호출
5. 다시 gen()이 처음부터 시작되지 않고 yield 이후부터 시작(여기선 i += 1)
6. While 내부에 있기 때문에 yield를 다시만나 증가한 값을 전달
7. 반복…

# generator expression
- List comprehension과 비슷함 : [ ]를 사용
- Generator function 쉽게 사용하기 위함 : ( )를 사용

```python
# List comprehension
[i for i in range(10) if i % 2]
# [1, 3, 5, 7, 9]

# Generator expression
(i for i in range(10) if i % 2)
# <generator object <genexpr> 0x0168F810>
```

# 활용
- Lazy evaluation

```python
def sleep_func(x):
print "sleep..."
time.sleep(1)
return x
```

위와 같은 함수가 있을 때, List와 Generator의 차이는

```python
# list 생성
list = [sleep_function(x) for x in range(5)]

for i in list:
print i

# 결과
sleep...
sleep...
sleep...
sleep...
sleep...
0
1
2
3
4
```

```python
# generator 생성
gen = (sleep_function(x) for x in range(5))

for i in gen:
print i

# <결과>
sleep...
0
sleep...
1
sleep...
2
sleep...
3
sleep...
4
```
- List comprehension은 List의 모든 값을 먼저 수행
	- 함수의 수행시간이 길거나 List값이 매우 큰 경우 부담
- Generator의 경우 생성 시에는 실제 값을 로딩하지는 않음
	- 필요할 때 하나씩 수행하며 값을 불러온다
> **수행시간이 긴 연산을 필요한 순간까지 늦출 수 있다**
