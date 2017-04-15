---
layout: post
title: 얕은복사 & 깊은복사
description: "Difference of Swallow Copy and Deep Copy"
tags: [OOP]
---
# 복사와 참조
-  a = b 같은 대입이 이루어 질 경우 b에 대한 새로운 참조생성
- 원시자료형이나 문자열의 경우에는 **immutable**이기 때문에 복사본이 생성되는 것 처럼 작동
	
- 그 외의 객체의 경우

```
A = [1,2,3,4]
B = A
B is A : true
B[2] = -100
A : [1,2,-100,4] // A도 함께 변경됨
```
	
# 얕은복사(Swallow Copy)
- 새로운 객체를 생성하지만 내부는 원래 객체의 참조로 채워진다

```
A = [1,2,[3,4]]
B = list(A) : 얕은 복사본 생성
B is A : false
	
B.append(100)
B : [1,2,[3,4],100]
A : [1,2,[3,4]] // A는 변하지 않음
	
B[2][0] = -100
B : [1,2,[-100,4],-100]
A : [1,2,[-100,4]] // A도 변화함
```
	
# 깊은복사(Deep Copy)
- 새로운 객체를 생성하고 원래 객체가 담고 있던 모든 객체를 재귀적으로 복사
- Python에서의 깊은 복사 : copy.deepcopy()를 이용

```python
import copy

A = [1,2,[3,4]]
B = copy.deepcopy(A)
B[2][0] = -100
B : [1,2,[-100,4]]
A : [1,2,[3,4]] // A는 변화하지 않음
```

# 번외
## Java에서는...
- Primitive type의 경우
- 변수 자체의 값을 가지기 때문에 그대로 복사
	
- Object class를 상속하는 모든 Obj의 경우
- Reference값 : 주소 만을 복사하게 되어 같은 참조를 가진 두개의 변수생성
- 한쪽에서 데이터를 변경하면 다른 한쪽도 변경되는 데이터를 가짐
> 얕은복사 (Shallow copy)
	
- 사용자 정의 Class Object가 모든 값을 복사함
> 깊은복사 (Deep Copy)

- 사용자 정의 Class에 Cloneable interface를 implement 한 후,	Object clone() method를 override하면 된다.
	
```java
public class route implements Cloneable {
    ArrayList<Integer> path;
    double cost;
    …
    public route clone() throws CloneNotSupportedException {
        route route = (route) super.clone(); // Cloneable을 통한 똑같은 객체 복사
        route.path = (ArrayList<Integer>) path.clone();
        route.cost = this.cost;
        return route; // 속성까지 부여하고 반환
    }
    …
}
```
	
- Collection은 내부에 clone()이 구현되어있으므로 복사 후 객체를 반환하면 됨.
- 속성에 사용자 정의 Class가 있을 경우, 그것도 clone구현하여 복사