---
layout: post
title: 테스트 주도 개발 접근법
description: "Access to TDD"
tags: [TDD]
---
## 접근법
1. **기능 테스트**를 작성해서 사용자 관점의 새로운 기능성 정의
2. **기능 테스트**가 실패하고 나면 어떻게 작성해야 테스트를 통과할지 (또는 적어도 현재 문제를 해결할 수 있는 방법)을 생각. 
3. 이 시점에서 하나 이상의 **단위 테스트**를 이용하여 어떻게 코드가 동작해야하는지 정의 (기본적으로 모든 코드가 (적어도) 하나 이상의 **단위 테스트**에 의해 테스트
4. **단위 테스트**가 실패하고 나면 **단위 테스트**를 통과할 수 있을 정도의 최소한의 코드작성
5. **기능 테스트**를 재실행해서 통과/동작하는지 확인. 
6. 완전해질때까지 반복(새로운 **단위 테스트**를 작성해야 할 수도있다.)

## 단위 테스트 : 코드 주기
- **단위 테스트**의 실행으로 어떻게 실패하는지 확인
- 현재의 실패 테스트를 수정하기 위한 최소한의 코드 변경
> 작은 작업단위로 나눠서 코드를 변경