---
title: 複製文檔到 Virtualbox
date: 2017-11-06 09:48:00
tags: 
- Virtualbox
categories:
- 筆記
---
## 安裝Guest-Additions
虛擬機內安裝dkms：
```
sudo apt-get install dkms
```
Virtualbox選擇Guest-Additions安裝：
* 裝置 -> 插入Guest-Additions
* 自動安裝（會需要sudo權限）

## 設定Virtualbox

### 共用剪貼簿
* 裝置 -> 共用剪貼簿 -> 雙向

### 拖曳傳輸檔案
* 裝置 -> 拖放 -> 雙向
