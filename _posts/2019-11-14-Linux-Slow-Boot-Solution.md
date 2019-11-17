---
layout: post
title: 우분투의 느린 부팅시간을 개선해보았다
description: "Soulution of slow linux boot"
tags: [Linux, Ubuntu]
---

# 개요

윈도우와 듀얼부팅 셋팅이 되어 있는데, 어느날부터 윈도우 부팅시간은 그대로지만 우분투 부팅시간이 3분이상 걸리는 현상이 생김.

# 원인파악

1. 부팅시간 확인

```bash
# 부팅시간 확인
$ systemd-analyze
Startup finished in 4.499s (kernel) + 3min 6.186s (userspace) = 3min 10.686s
```

2. 서비스 부팅 소요시간

```bash
$ systemd-analyze blame
3min 19.476s apt-daily-upgrade.service
         26.877s apt-daily.service
          2.302s motd-news.service
          ...
```

- 여기서는 `apt-daily-upgrade.service`, `apt-daily.service` 에 문제가 있었음
- 어쨋든 문제의 원인을 찾아냄

3. 서비스 실행주기 수정

- [Debian 버그](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=844453)

```bash
$ sudo systemctl edit apt-daily.timer
# apt-daily timer configuration override
[Timer]
OnBootSec=15min
OnUnitActiveSec=1d
AccuracySec=1h
RandomizedDelaySec=30min
```
