---
title: CTFd 架設筆記
date: 2017-12-08 20:30:00
tags: 
categories:
- 筆記
---
## 使用環境
* ubuntu 16.04
* python 2.7.x
* nginx
* mysql

## 安裝套件
```
sudo apt-get install python-mysqldb
sudo pip install Flask
sudo pip install uwsgi -I
```

## 安裝CTFd
### 下載CTFd
```
git clone https://github.com/isislab/CTFd.git
cd CTFd
```
### 安裝CTFd
```
./prepare.sh
```
### 將CTFd連上MySQL
在資料庫中建立一個資料庫CTFd，不用建立內容，在CTFd第一次執行後會自動建立
修改CTFd目錄下的 `config.py` (此檔案在CTFd中的CTFd)
```
SQLALCHEMY_DATABASE_URI = 'mysql://[SQLaccount]:[SQLpasswd]@localhost/CTFd?charset=utf8mb4'
```

## 啟動CTFd
執行 `serve.py` 即可在 `127.0.0.1:4000` 看到你的站已經架起來了

### 設定Nginx
因為單純用flask框架開啟CTFd效能並不好，因此建議使用Apache或是Nginx，以下採用Nginx作為示範。
因為是使用 `uwsgi` 作為串接，因此需要更改Nginx的設定。
```
location / {
    include uwsgi_params;
    uwsgi_pass unix:/tmp/uwsgi.sock;
}
```

### 啟動CTFd
```
uwsgi -s /tmp/uwsgi.sock -w "CTFd:create_app()" --chmod-socket=666
```

## 其他
若有較大的檔案需要傳至CTFd，可增加Nginx設定。
```
client_max_body_size [size];
```

## 備份方法
* 直接備份資料庫
* 登入管理員帳號 -> Admin -> Config -> Backup
