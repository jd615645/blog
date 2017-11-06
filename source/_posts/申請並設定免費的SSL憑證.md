---
title: 申請並設定免費的SSL憑證(Let’s Encrypt)
date: 2017-11-06 11:40:00
tags: 
- SSL
- HTTPS
- Let’s Encrypt
categories:
- 資安
---
# Setup SSL
Let’s Encrypt是一個非營利組織，提供免費的TLS/SSL 憑證，透過Cerbot這個自動化工具，就可以讓網站使用HTTPS加密網頁，也能自動的更新憑證。


## 下載SSL憑證

Let’s Encrypt可以手動更新也可以透過指令工具進行下載，這邊使用官方建議的Certbot自動畫工具來下載。

從 Certbot 官方網站下載  `certbot-auto` ，設定執行權限，並將它放在 `/opt` 下：

```
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
mkdir /opt/letsencrypt
mv certbot-auto /opt/letsencrypt/
```

執行 `certbot-auto`，讓它自動安裝所有相依套件（需要root權限）：

```
/opt/letsencrypt/certbot-auto
```

接著我們要透過 `webroot` 的方式，使用既有的 nginx 網頁伺服器來向 Let’s Encrypt 取得憑證，而在認證的過程會需要在網頁根目錄中建立一個 `.well-known/acme-challenge/` 目錄，讓 Let’s Encrypt 的伺服器來讀取其中的內容。
一般的 nginx 伺服器通常會設定把句點開頭的隱藏檔案都擋掉，遇到這樣的狀況就會無法進行認證，這時候可以再加一小段設定，讓 `.well-known/acme-challenge/` 目錄可以被正常讀取：

```
location ^~ /.well-known/acme-challenge/ {
  # the usual settings
}

location ~ /\. {
        deny all;
}
```

使用 `certonly` 功能下載憑證，此處會要輸入Email跟使用條款確認，若無錯誤即可取得憑證：

```
/opt/letsencrypt/certbot-auto certonly --webroot -w [網頁路徑] -d [domain]
```

成功取得的憑證儲存在 `/etc/letsencrypt/live/`[domain] 目錄之下，其中 `fullchain.pem` 就是 nginx 需要憑證，而 `privkey.pem` 則是私鑰


## **設定 nginx 伺服器使用 SSL 憑證**

若要啟用nginx的HTTPS功能需要再增加一些設定
```
server {
  # listen HTTPS port 443
  listen 443;
  # IPv6 listen HTTPS port 443
  listen [::]:443;
  
  server_name [請自行填入server_name];

  root [網頁路徑位置];
  index index.php index.html index.htm;
  
  # 啟用 SSL
  ssl on;
  
  # 其他 SSL 選項
  ssl_session_timeout 5m;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # omit SSLv3 because of POODLE (CVE-2014-3566)
  ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
  ssl_prefer_server_ciphers on;
}
```

設定完畢後可使用指令測試設定檔是否正確：
```
service nginx -t
```

若設定無誤，重新載入設定檔即可：
```
serivce nginx reload
```

若用Cloudflare代管的話請記得將Crypto中的設定改為Full或是Full(strict)，讓主機到Cloudflare這段過程是加密的。（**在瀏覽器端看到的簽證會是Cloudflare，若要測試是否有成功可先讓DNS設定不要過CDN來查看憑證狀況）**

以下是Cloudflare的SSL幾個選項的介紹：

* **Flexible SSL**：
  * 部分SSL加密連線，你不必擁有SSL證書。免技術直接套用Cloudflare免費SSL。訪客連線到Cloudflare是採用加密連線，從Cloudflare到主機則不走加密連線。

![Flexible SSL](https://www.cloudflare.com/img/products/ssl/flexible-ssl.svg)

* **Full SSL**：
  * 全程SSL加密連線，你必須擁有一個SSL證書在你的主機上，不過Cloudflare並不會檢查你的SSL證書是自己簽署或第三方公正單位發下來的。
* **Full SSL(Strict)**：
  * 全程使用SSL加密連線，你必須擁有一個SSL證書在你網站上，而且Cloudflare會檢查你主機端的SSL證書是否為第三方公正單位簽署(不能使用自己簽署的)。

![Strict SSL](https://www.cloudflare.com/img/products/ssl/full-ssl-strict.svg)

## 自動更新憑證

Let’s Encrypt 的憑證使用期限為3個月，在憑證到期前的1個月可以使用 `certbot-auto` 來更新憑證，在實際更新之前我們可以加入 `--dry-run` 參數，先進行測試：
```
/opt/letsencrypt/certbot-auto renew --dry-run
```

若測試無誤可執行下列指令：
```
/opt/letsencrypt/certbot-auto renew --quiet --no-self-upgrade
```

可以寫一支shell放在 `/opt/letsencrypt/renew.sh` 中，方便使用：
```
#!/bin/sh
/opt/letsencrypt/certbot-auto renew --quiet --no-self-upgrade --post-hook "service nginx reload"
```

`--post-hook` 可以讓憑證更新完後，可以自動重新載入 nginx 伺服器的設定，讓憑證生效。

接著把這個 `/opt/letsencrypt/renew.sh` 指令稿寫進 crontab 中：
```
# m h  dom mon dow   command
30 2 * * 0 /opt/letsencrypt/renew.sh
```

官方的建議是這個指令可以一天執行兩次，讓伺服器的憑證隨時保持在最新的狀態，這裡我是設定讓伺服器每週日凌晨兩點半進行憑證的檢查與更新，若要其他時間可在自己更改crontab或是用 [此工具](https://crontab.guru/every-5-minutes) 參考設定做調整設定。Certbot 只有在憑證到期前一個月才會進行更新，如果憑證尚未到期，就不會更新。

## 參考資料

* https://blog.gtwang.org/linux/secure-nginx-with-lets-encrypt-ssl-certificate-on-ubuntu-and-debian/
* https://sofree.cc/cloudflare-free-ssl/

