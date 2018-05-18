---
title: 在VScode上使用FiraCode
date: 2018-05-18 18:20:00
tags: 
categories:
- 筆記
---
FiraCode是一款特殊字體可以，其特殊之處是會將 `!=` `===` 等符號合併起來，使用起來非常一目了然，十分推薦使用。

使用情況如下圖所示：

![](https://raw.githubusercontent.com/tonsky/FiraCode/master/showcases/all_ligatures.png)

## 使用方法
1. 下載 [FiraCode](https://github.com/tonsky/FiraCode) 並解壓縮
2. 安裝字體(Linux下請將字體複製到 `/usr/share/fonts` 目錄下的對應資料夾)
3. 至VScode中的 `檔案 -> 喜好設定 -> 設定` ，在右側編輯欄位加入以下資訊後存檔
```
"editor.fontFamily": "Fira Code",
"editor.fontSize": 14,
"editor.fontLigatures": true
```
4. 關閉並重新開啟VScode就完成套用啦 ｡:.ﾟヽ(*´∀`)ﾉﾟ.:｡
