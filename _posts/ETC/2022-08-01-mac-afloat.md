---
layout: post
title: '[MacOS] 창 항상 위로 AfloatX'
category: etc
tags: [mac, afloat]
comments: true
---

# Environment
- MacOS Monterey 12.5

# System Integrity 일부 비활성화
- 부팅 시 command + R 눌러 Recovery mode 진입
- 터미널 열고 아래 명령어 실행 후 재부팅

```sh
$ csrutil enable --without fs --without nvram --without debug
```

# Library Validation 비활성화

```sh
$ sudo defaults write /Library/Preferences/com.apple.security.libraryvalidation.plist DisableLibraryValidation -bool true
```

# MacForge 설치
- MacForge 다운로드 후 압축 풀고 **AfloatX** 설치 후 재부팅
- 현재 **Afloat**는 불안정하고 버전업에 따른 유지 보수 안됨, 대신 **AfloatX**을 설치함

```sh
# MacForge 다운로드
$ wegt https://github.com/w0lfschild/app_updates/raw/master/MacForge1/MacForge.zip
```

# 서비스 등록 후 단축키 등록
- 아래 Apple Script로 workflow 만들고, 단축키 등록

```applescript
on run
	-- get active application
	tell application "System Events" to set activeApp to name of first application process whose frontmost is true
	
	-- right click that application in the dock
	tell application "System Events" to tell UI element activeApp of list 1 of process "Dock"
		perform action "AXShowMenu"
		click menu item "AfloatX" of menu 1
		click menu item "Float Window" of menu 1 of menu item "AfloatX" of menu 1
	end tell
end run
```