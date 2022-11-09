---
layout: post
title: '[MacOS] Finder에서 SFTP 연결'
category: etc
tags: [mac, sftp]
comments: true
---

# Environment
- MacOS Monterey 12.5

# sshfs 설치

```sh
$ brew install --cask macfuse
$ brew install gromgit/fuse/sshfs-mac
$ brew link --overwrite sshfs-mac
```

# sshfs mount

```sh
sshfs <REMOTE_USER_NAME>@<REMOTE_IP_ADDRESS>:<REMOTE_PATH> <LOCAL_PATH> -p <REMOTE_PORT> -ovolname=<VOLUME_NAME>
```

# sshfs umount

```sh
$ umount -f <LOCAL_PATH>
```