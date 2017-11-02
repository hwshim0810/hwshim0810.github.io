---
layout: post
title: Docker 설치과정 및 기초명령어 (Ubuntu 16.04)
description: "Docker install"
tags: [Linux, Docker, Installation]
---
## 설치 및 권한부여
```bash
$ curl -s https://get.docker.com/ | sudo sh

# 현재 사용자에게 권한부여
$ sudo usermod -aG docker $USER
# 특정사용자에게 권한부여
$ sudo usermod -aG docker USER-NAME
# 재로그인 시 적용
```

## 설치확인
```bash
$ docker version
```

---

## 컨테이너 실행
`docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`

| 옵션  | 설명  |
|---|---|
| -d  | detached mode (백그라운드 mode)  |
| -p  | Host 와 컨테이너의 Port 연결(포워딩)  |
| -v  | Host 와 컨테이너의 디렉토리를 연결(마운트)  |
| -e  | 컨테이너 내부의 환경변수 설정  |
| -name  | 컨테이너 이름설정  |
| -rm  | 프로세스 종료시 컨테이너 자동제거  |
| -it  | == -i -t (터미널 입력을 위한 옵션) |
| -link  | 컨테이너 연결[컨테이너명:별칭]  |


## 실행 예
```bash
# 우분투 컨테이너 생성 및 터미널 진입
$ docker run --rm -it ubuntu:16.04 /bin/bash

# MySQL 컨테이너
$ docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \ # 암호없는 루트계정 생성
  --name mysql \
  mysql:5.7

$ mysql -h127.0.0.1 -uroot
mysql> ...
```

## 다른 명령어
### 1. 컨테이너 목록 확인하기  

> `docker ps [OPTIONS]`  
	- 기본은 실행중인 컨테이너만 보여줌  

| 옵션  | 설명  |
|---|---|
| -a (--all)  | 종료된 컨테이너까지 출력  |
| -n (--last) n | 최근 생성된 n개의 컨테이너 |

### 2. 컨테이너 중지하기
> `docker stop [OPTIONS] CONTAINER [CONTAINER...]`  
    - 실행 중인 컨테이너를 하나 혹은 여러개(띄워쓰기) 중지

- 컨테이너의 ID혹은 이름
- ID는 겹치지 않는 앞부분 일부만으로도 가능

### 3. 컨테이너 제거하기
> `docker rm [OPTIONS] CONTAINER [CONTAINER...]`  
    - 종료된 컨테이너를 완전히 제거  
    - 컨테이너의 ID혹은 이름  

####  @중지된 컨테이너를 한번에 삭제
> `docker rm -v $(docker ps -a -q -f status=exited)`

### 4. 이미지 목록 확인하기
> `docker images [OPTIONS] [REPOSITORY[:TAG]]`  
    - 다운로드한 이미지 목록

### 5. 이미지 다운로드
> `docker pull [OPTIONS] NAME[:TAG|@DIGEST]`  
    - 최신버전으로 다시 업데이트해야하는 경우

### 6. 이미지 삭제
> `docker rmi [OPTIONS] IMAGE [IMAGE...]`  
    - 이미지 ID를 이용한 삭제 (ID는 목록으로 확인)

### 7. 컨테이너 로그확인
> `docker logs [OPTIONS] CONTAINER`  
    - 컨테이너의 표준 출력, 표준 에러를 확인  
    - 컨테이너의 프로그램의 로그설정을 파일 -> 표준출력으로 바꿔야 이것을 이용할 수 있음

| 옵션  | 설명  |
|---|---|
| -f | 실시간으로 확인 |
| --tail | 마지막 10줄만 확인 |

### 8. 컨테이너 명령어 실행
> `docker exec [OPTIONS] CONTAINER CMD [ARG...]`  
    - 실행중인 컨테이너에 명령

```bash
# MySQL 예제
$ docker exec -it mysql /bin/bash
$ mysql -uroot
```

## 컨테이너 업데이트
1. 새 버전의 이미지 pull
2. 기존 컨테이너 stop -> rm
3. 새로운 컨테이너 run

- 컨테이너 삭제시 컨테이너 내부의 파일이 삭제됨
- 데이터를 유지해야하는 경우 외부 스토리지 이용
    - EX) AWS S3
- 또는 데이터 볼륨을 컨테이너에 추가해서 사용하는 방법

```bash
# Host 의 디렉토리를 마운트하여 사용하는 방법
# MySQL의 경우
$ docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  -v [Mount 할 HOST경로 ex)/home/...]:/var/lib/mysql \
  mysql:5.7
```
