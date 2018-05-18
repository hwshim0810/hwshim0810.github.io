---
layout: post
title: 백분위수(Percentile) 파이썬으로 구해보기
description: "Calculate percentile in python"
tags: [Python, DataScience]
---

## 분위수 (Quantile)
  - `자료의 크기순서에 따른 위치값`

- `평균`만으로 통계를 대표하는게 아닌 평균과 분위값이 같이 고려되어야 하는 경우 사용
  - 자료의 형태가 정규분포를 벗어나는 경우가 많을 때
  - 산포가 매우 큰 경우
  - 상하위 부분에서 극단적인 치우침이 있어서 극단값이 중요한 의미를 지니는 경우

- `X 분위값` 이란 자료값 중 X%가 그 값보다 작거나 같게 되는 값

## 백분율과 백분위/백분위수

- 백분율 (Percent)
  - `비율을 나타내는 방식`
  - 전체의 수량을 100이라 할 때, 생각하는 수량이 그 중 몇이 되는가를 가리키는 수(퍼센트 %)로 나타냄

- 백분위수 (Percentile)
  - `크기가 있는 값들을 순서대로 나열했을 때 백분율로 나타낸 특정 위치의 값`
  - 100개의 값을 가진 어떤 자료의 20 백분위수는 그 자료의 값들 중 20번째로 작은 `값`을 뜻한다. 50 백분위수는 중앙값과 같다
  - ex) 1사분위 수 = 25% 백분위 수

- 백분위 (Percentile rank)
  - `특정집단의 점수분포상에서 한 개인의 상대적 위치를 알 수 있는 유도점수`
  - 자료의 특정값이 전체에서 어느 `위치`에 나타내고자 할 때 사용


### 나만의 요약
- 평균의 함정에서 벗어나 값의 범위를 측정해보기에 적절한 듯 함


## Python Code
- Numpy 이용

```python
import numpy as np

a = np.array([1,2,3,4,5])
p = np.percentile(a, 50) # return 50th percentile, e.g median.
print(p)

> 3.0
```

- [참조: scipy](https://docs.scipy.org/doc/numpy/reference/generated/numpy.percentile.html)