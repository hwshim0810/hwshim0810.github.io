---
layout: post
title: Project fork 와 Sync 맞추기
description: "How to sync local project to forked project"
tags: [Git, Opensource]
---
## 순서
1. Github 에서 fork 로 복사하고 싶은 프로젝트를 가져온다
2. git clone [내 계정의 Fork해온 저장소 주소] 으로 내 저장소에 fork 해온것을 clone 한다.
3. git remote add --track master upstream(일반적으로 원격저장소를 의미) [원본저장소 주소]
4. git fetch upstream (일반적으로 원본 master와 연결된 브랜치가 생성됨)
5. git merge upstream/[나의 로컬브랜치] 로 Sync