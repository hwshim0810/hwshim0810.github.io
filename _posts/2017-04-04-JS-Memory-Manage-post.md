---
layout: post
title: Javascirpt Memory Manage
description: "JS Memory Manage"
tags: [Javascript]
---
### Js에서는 값을 선언할 때 메모리를 할당한다.
- 함수 호출을 통한 메모리 할당 
> dom element 생성 등

### Js에서 문자열은 immutable값
- substring(0,3) 같은 경우, 메모리가 새로 할당되지 않고 단순히 [0,3]이라는 범위만 저장

***
## 가비지 콜렉션
- 모든 Js object는 prototype을 암시적으로 참조하고 그 obj의 속성을 명시적으로 참조한다.
- A라는 메모리를 통해 B라는 메모리에 접근할 수 있다면 B는 A에 참조된다.

### 참조-세기 가비지 콜렉션 (Reference-counting)
- 가장 무난한 알고리즘
- *더 이상 필요없는 Object*를 **어떤 다른 Object도 참조하지 않는 Object**라고 정의.
- 어떤 Object를 참조하는 다른 Object가 하나도 없다면 그 Object에 대하여 수행.
- 두 Obj가 서로를 참조할 경우 둘다 필요없어졌더라도 가비지컬렉션 수행불가.

### 표시하고-쓸기 (Mark-and-sweep) 알고리즘
- *더 이상 필요없는 Obj*를 **닿을 수 없는 Obj**로 정의.
- Roots라고 불리는 Obj의 집합(js에서 전역변수)을 가짐.
- Roots로 부터 시작하여 roots가 참조하는 obj, 참조하는 obj가 참조하는…  
연관된 obj를 **닿을 수 있는 Obj**로 표시.
- 닿을 수 없는 obj에 대해 가비지 콜렉션 수행
- 참조-세기 알고리즘보다 효율적
1. "참조되지 않는 obj"는 모두 "닿을 수 없는 obj"이지만 역은 성립하지 않기 때문에
2. 반례는 순환참조하는 obj
	
- 최신 브라우저들은 Mark-and-sweep알고리즘을 사용.