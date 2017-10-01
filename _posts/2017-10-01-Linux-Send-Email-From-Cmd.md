---
layout: post
title: 리눅스 CMD 에서 Email 보내기
description: "Send email from linux cmd"
tags: [Linux]
---
# 선행조건
- sudo apt-get install mutt

# 명령어
> 본문내용 CMD에서 작성  
- echo '내용' \| mutt -s '제목' 주소@주소 -a 첨부파일

> 본문내용 외부파일 사용
- mutt -s '제목' 주소@주소 < 내용.txt -a 첨부

- 여러명한테 보낼 때 주소뒤에 주소계속 연결가능
- -c 주소@주소 : 참조
- -b 주소@주소 : 숨은참
