---
title: TDOH-PIPE筆記 WHOIS
date: 2017-10-10 12:00:00
tags: 
- TDOH-PIPE
- WHOIS
categories:
- TDOH-PIPE
---
WHOIS(讀作「Who is」)，是用來查詢IP位置或是域名(Domain)的所有者資訊的傳輸協定。此通訊協定是用人類可讀的格式進行儲存跟傳遞。早期查詢是用命令提示介面(Command Line)，但現在有不少基於網頁介面的簡化線上查詢工具。WHOIS protocol的協議記錄在 RFC 3912，通常使用43port進行傳輸。

WHOIS查詢工具：(https://www.whois.net/)
查詢結果如下示意(因為在gandi有勾選隱藏資料，所以以下的部分資料是網域註冊商的)

![](https://i.imgur.com/RKxe4SB.png?1)

## Servers

WHOIS server 由各Regional Internet Registries(RIP，譯為”區域網際網路註冊管理機構”，管理世界上特定區域的internet資源的組織)負責，可用來進行查詢確定負責特定資源的互聯網服務提供商(提供ADSL、行動網路服務之公司，如種花電信)。

雖然一些頂級域名(國家或地區頂級域名等)有使用一些查詢該domain的方法，但查詢DNS domain負責的是哪個WHOIS伺服器的標準方式目前還沒有一個規範的方法。