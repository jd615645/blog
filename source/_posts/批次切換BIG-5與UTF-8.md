---
title: 批次切換BIG-5與UTF-8
date: 2017-04-03 12:00:00
tags: 
- big5
- utf8
- bash tool
categories:
- Linux
---
檔案編碼格式不統一一直是令人抓狂的問題，因此可以用一個在Linux的指令套件 iconv 來進行轉檔，但是單純下指令一次只能轉換一個檔案，因此寫了一個小小的shell scripts來進行批次轉檔。

> iconv是一個電腦程式以及一套應用程式編程介面的名稱。它的作用是在多種國際編碼格式之間進行文字內碼的轉換。
> 支援的內碼包括：
> * Unicode相關編碼，如UTF-8、UTF-16...
> * 各國採用的ANSI編碼，其中包括GB2312、BIG5等中文編碼方式

## Install(debian, ubuntu)
```
apt-get install iconv 
```

## Example
### BIG-5 csv轉UTF-8 csv
```
iconv -f BIG-5 -t UTF-8 big5.csv > utf8.csv
```

### UTF-8 csv轉BIG-5 csv
```
iconv -f UTF-8 -t BIG-5 utf8.txt > big5.txt
```

### 批次轉換目錄下的BIG-5 csv轉UTF-8 csv
```bash
for file in *.csv; do 
    iconv -f big-5 -t utf-8 "$file" -o "${file%.csv}.utf8.csv"
done 
```

## 參考資料
* [Wikipedia: iconv](https://en.wikipedia.org/wiki/Iconv)
* [iconv 指令轉換文字檔編碼（Big5 轉 UTF8、UTF8 轉 Big5 ）](https://blog.gtwang.org/tips/iconv-convert-text-big5-between-utf8-encoding/)