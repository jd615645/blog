---
title: 更改git協議在https與ssh間切換
date: 2018-05-05 20:00:00
tags: 
- git
categories:
- 筆記
---

## 切換 remote URLs 從ssh至https
* 開啟terminal，並切換至專案目錄下
8 列出現有remote，取得目前需要更改remote的repo名稱
```
$ git remote -v
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)  
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```
用 git remote set-url 將remote's URL從https至ssh
```
$ git remote set-url origin git@github.USERNAME/REPO.git
```
確認是否成功
```
$ git remote -v
origin  https://github.com/USERNAME/OTHERREPOSITORY.git (fetch)  
origin  https://github.com/USERNAME/OTHERREPOSITORY.git (push)  
```

## 切換 remote URLs 從ssh至https
開啟terminal，並切換至專案目錄下
列出現有remote，取得目前需要更改remote的repo名稱
```
$ git remote -v
origin  https://github.com/USERNAME/REPOSITORY.git (fetch)  
origin  https://github.com/USERNAME/REPOSITORY.git (push)  
```
用 git remote set-url 將remote's URL從ssh至https
```
$ git remote set-url origin https://github.com/USERNAME/REPO.git
```
確認是否成功
```
$ git remote -v
origin  git@github.com:USERNAME/OTHERREPOSITORY.git (fetch)  
origin  git@github.com:USERNAME/OTHERREPOSITORY.git (push)  
```
