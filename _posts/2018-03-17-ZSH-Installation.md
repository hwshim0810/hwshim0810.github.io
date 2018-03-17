---
layout: post
title: oh-my-zsh 이용하여 터미널 커스텀하기
description: "Apply oh-my-zsh for terminal custom"
tags: [Linux, Installation]
---

### 설치과정
```bash
# zsh 설치
$ sudo apt-get install zsh

# 기본 쉘 수정
$ chsh -s `which zsh`
# 로그아웃 후 재시작 ...

# 쉘확인
$ echo $SHELL
/usr/bin/zsh

# Oh My Zsh 설치
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

### 테마적용
- [테마목록](https://github.com/robbyrussell/oh-my-zsh/tree/master/themes)

```bash
# .zshrc 에서 설정값 수정
# 아래는 기본 테마설정
ZSH_THEME="robbyrussell"
```

### 터미널 글꼴깨짐 수정
```bash
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
$ ./install.sh

# 터미널 프로파일 설정에서 
# ... forPowerline 폰트로 변경
```

### 색상변경
- `~/.oh-my-zsh/themes/[사용중인 테마파일]`
- 기본색상: white, black, red, blue, green, yellow, cyan, magenta
- {% raw %}%{%F{0~255}%} 또는 %{%K{0~255}%}{% endraw %}를 통해 설정
- 추가색상 [참조](https://jonasjacek.github.io/colors/)

### Vscode 터미널 폰트깨짐 수정사항 적용
- 사용자 설정 덮어쓰기
`"terminal.integrated.fontFamily": ... for Powerline"`

### 사용자 이름줄이기
```bash
# .zshrc 에 추가
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default {% raw %}"%(!.%{%F{yellow}%}.){% endraw %}$USER"
  fi
}
# 혹은
# 아래 설정은 유저이름을 모두 숨김처리
prompt_context(){} 

```