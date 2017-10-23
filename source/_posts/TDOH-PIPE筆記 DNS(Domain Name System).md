---
title: TDOH-PIPE筆記 DNS(Domain Name System)
date: 2017-10-10 12:00:00
tags: 
- TDOH-PIPE
- DNS
categories:
- TDOH-PIPE
---
DNS(Domain Name System，網域名稱系統)，是一項網際網路的服務，其功能是將域名和IP位置相互對應的一個分散式資料庫，可以讓人更方便的存取網際網路。每個domain長度限制是63字元總域名長度不能超過253字元。

早期的domain僅限於ASCII字元的一個子集，不過2008年ICANN(網際網路名稱與數字位址分配機構)通過一項決議，可將其他語言用於頂級域名的字元。使用基於Punycode碼的IDNA系統，可以將unicode字串對映為有效的DNS字元集。不過若用其他語言文字作為域名可能會造成難以輸入等問題，若要使用請注意。


## 解析域名

以下用wiki的範例說明

zh.wikipedia.org(domain) <--> 208.80.154.225(IP address)，此組域名與 IP位置相互對應，如同住址一般可以幫我們指到對應經緯度的住家。

以 `zh.wikipedia.org` 查詢舉例
用戶使用  `query zh.wikipedia.org` 到DNS server，DNS server會先檢查自身快取，若有則直接返回結果，若無則進行以下步驟：

1. DNS server 向 root name server 送出查詢指令 `query zh.wikipedia.org` ，root name server會返回 `.org` 此頂級域名的權威域名server 位址
2. DNS server 會向頂級域名的權威域名server 位置送出查詢指令 `query zh.wikipedia.org` ，得到 `.wikipedia.org` 域的權威域名伺服器位址
3. DNS server 會向 `.wikipedia.org` 域的權威域名伺服器位址送出查詢指令 `query zh.wikipedia.org` ，得到主機zh的A記錄，存入自身快取並返回給用戶端。