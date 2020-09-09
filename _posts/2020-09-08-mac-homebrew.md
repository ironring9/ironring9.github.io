---
title: "Homebrew란?"
categories: 
  - Mac
toc: true
---

홈브류는 Mac용 패키지 관리자입니다. OS패키지 관리자를 통해 프로그램을 관리할 수 있습니다.
프로그램 관리를 터미널로 완전히 제어할 수 있어서 컴퓨터를 깔끔하게 관리할 수 있습니다.

# Homebrew 설치
![mac-homebrew-img001]({{site.url}}/assets/images/mac-homebrew-img001.png)

[homebrew 홈페이지](https://brew.sh/index_ko)에서 명령을 복사합니다.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
터미널에 복사한 명령을 실행합니다.

# Homebrew 활용하기

homebrew를 사용하여 **cask** 패키지를 설치해보겠습니다.

**cask**는 Safari, Chrome, Word 등과 같이 그래픽을 통해 작업하는 
프로그램을 설치할 수 있게 해주는 패키지입니다.

## 패키지 설치

```
brew install cask
```
위 명령어로 cask 패키지 설치합니다.

```
brew list
```
위 명령어를 통해 설치된 패키지를 확인할 수 있습니다.

## 패키지 삭제

```
brew uninstall cask
```

# 자주 사용하는 명령 #
brew ~ : 커맨드 라인 프로그램 (c, java, python 같은..)

brew cask ~ : GUI 프로그램 (Safari, Chrome, Word 같은..)

brew update : 홈브류 최신버전으로 업데이트

brew upgrade [프로그램명]: 홈브류에 설치된 프로그램 최선버전으로 업데이트

brew search [프로그램명] : 홈브류를 통해 설치 가능한 프로그램 찾기

brew cask list : 홈브류에 설치된 그래픽을 통해 작업하는 프로그램 목록 (Safari, Chrom, Word와 같은 일반적인 앱)

brew cask install [프로그램명] : 프로그램 설치

brew cask remove [프로그램명] : 홈브류에 설치된 프로그램 삭제

brew cleanup : 업데이트 후 필요없는 이전 버전의 패키지 삭제


