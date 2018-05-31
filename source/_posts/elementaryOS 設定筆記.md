---
title: elementaryOS 設定筆記
date: 2018-05-31 17:30:00
tags: 
- Linux
categories:
- 筆記
---
## Add ppa
有時我們會想使用PPA來安裝一些別人打包好的套件，但是 elementaryos 團隊貌似因為某些安全性考量並未把 `software-properties-common` 包入系統，讓我們無法直接這麼作，但是我們依然能用指令將它裝回來
```
sudo apt install software-properties-common
```

## 安裝 elementary-tweaks
```
sudo add-apt-repository ppa:philip.scott/elementary-tweaks
sudo apt-get update
sudo apt-get install elementary-tweaks
```

## 安裝中文輸入
1.安裝Fcitx-nighty，因為支援度較高所以選擇他
```
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
```

2.安裝Fcitx，和google 拼音以及新酷音
```
sudo apt-get install fcitx fcitx-chewing
```

3.安裝其他必要元件
```
sudo apt-get install fcitx-table-all
```

4.使用 `im-config` 來管理輸入法
```
sudo apt-get install im-config
```
