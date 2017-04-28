---
layout: post
title: args와 kwargs의 사용
description: "Difference of args and kwargs"
tags: [Python]
---
# *args
> 함수로 전달 될 Parameter가 몇 개 일지 모르는 경우

- Tuple로 전달됨

```python
def print_param(*args):
    print(args)
    for pa in args:
        print(pa)

print_param('a', 'b', 'c', 'd')
#('a', 'b', 'c', 'd')
#a
#b
#c
#d
```

# **kwargs
> Parameter Name과 같이 보낼 수 있다

- Dictionary로 전달됨

```python
def print_param2(**kwargs):
    print(kwargs)
    print(kwargs.keys())
    print(kwargs.values())
    
    for name, value in kwargs.items():
        print("%s : %s" % (name, value))

print_param2(first='a', second='b', third='c', fourth='d')

#{'second': 'b', 'fourth': 'd', 'third': 'c', 'first': 'a'}
#['second', 'fourth', 'third', 'first']
#['b', 'd', 'c', 'a']
#second : b
#fourth : d
#third : c
#first : a
```

## 활용
- 둘을 같이써도 됨
	- 같이 사용시에는 *args가 **kwargs 앞에와야 함
- 이름이 있는 Parameter와 같이 사용해도 됨
	- 이름이 정의 된게 먼저와야 함
	

```python
def print_param3(*args, **kwargs):
    print(args)
    print(kwargs)

print_param3('a', 'b')
#('a', 'b')
#{}

print_param3(third='c', fourth='d')
#()
#{'fourth': 'd', 'third': 'c'}

print_param3('a', 'b', third='c', fourth='d')
#('a', 'b')
#{'fourth': 'd', 'third': 'c'}
```

***

```python
#이름이 정의되어 있음
def print_param4(a, b, c):
    print(a, b, c)

p = ['a', 'b', 'c']
print_param4(*p)
#a b c

p2 = {'c': '1', 'a': '2', 'b': '3'}
#정의된 함수의 파라미터명과 맞지 않거나 갯수가 다른 경우 error
print_param4(**p2) 
#2 3 1
```
