---
title: Node.js-使用forever運行Node.js服務
date: 2017-10-21 12:00:00
tags: [Node.js, Forever] 
---
Forever是用來使node.js程式可以在後台一直運作的指令工具，如果程式出錯它也會幫你自動重啟，對於一些網站服務而言非常的重要。
[Forever github 頁面](https://github.com/foreverjs/forever)

## 安裝Forever
用NPM來安裝Forever，建議使用全域安裝
```
npm install -g forever
```

## 使用forever運行node.js程式
進入要使用forever的資料夾，並值行以下指令
```
forever start -c "npm start" ./
```
若無錯誤訊息，代表就成功了，可以用以下指令來看看你的程式有沒有在上面列出來
```
forever list
```

## 常用指令
想確認你的程式的pid可以使用`forever list`查詢

後台啟動一個程式
```
forever start [pid]
```
停止一個後台運作中的程式
```
forever stop [pid]
```
停止所有後台運作中的程式
```
forever stopall
```
重新啟動一個後台運作中的程式
```
forever restart [pid]
```
重新啟動所有後台運作中的程式
```
forever restartall
```
列出所有後台運作中的程式
```
forever list
```

