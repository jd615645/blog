---
title: linux 編譯安裝git
date: 2018-05-05 20:00:00
tags: 
- git
categories:
- 筆記
---
作業系統：ubuntu 17.04

因為要使用git必須安裝，一些相依套件curl, zlib, openssl, expat, libiconv
```
$ sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext
```

為了能夠建立這些格式（doc、html、info）的文件，你還需要安裝這些額外的相依關係
```
$ sudo apt-get install asciidoc xmlto docbook2x
```

安裝好這些相依套件後即可下載最新的git原始碼進行編譯。可至 https://github.com/git/git/releases 下載最新版本，之後按照以下指令步驟進行：
```
$ tar -zxf git-2.0.0.tar.gz
$ cd git-2.0.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```
![](https://imgur.com/2aeN6Zk.png)

![](https://imgur.com/MI22J52.png)
