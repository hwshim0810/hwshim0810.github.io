---
layout: post
title: Python 패키지 만들기 간단요약
description: "Summary of making python package"
tags: [Python]
---

### setup.py
- 일반옵션 생략
- packages
  - 빌드에 포함할 Package 목록
  - 테스트나 Doc 등을 제외
- keywords
  - 프로젝트 관련 키워드
- python_requires
  - 지원하는 Python version 설정
  - ex) 2.7 only -> `=2.7`, Python 3버전 이상 -> `>=3`
- package_data
  - setup 에는 기본적으로 파이썬 파일만 빌드에 포함
  - 파이썬 파일이 아닌 외부파일을 포함시키기 위함
- zip_safe
  - package_data 설정시 필요: `False`
- classifiers
  - PyPi 에 등록될 메타 데이터
  - 실제 빌드에는 영향을 주지않음.

### setup.cfg
- setup.py 에 전달할 명령에 대한 옵션 기본값등

### MANIFEST.in
- 내부 패키지 디렉토리에는 들어있지 않지만 포함시키고 싶은 파일의 목록
- README, 라이센스 파일 등등...

```
include LICENSE
include README.md
...and more
```

## 배포

### 소스배포판
```
$ python setup.py sdist

# 설치하기 -> site-packages
$ pip install dist/[...].tar.gz
```

### Wheel
- Python 과 선택적으로 C 확장을 패키징하기 위함.
```
# 빌드
$ python setup.py bdist_wheel
```

### 배포하기
```
$ pip install twine

$ twine upload dist/[...].whl
```