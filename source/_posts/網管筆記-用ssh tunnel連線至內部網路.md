---
title: 網管筆記-用ssh tunnel連線至內部網路
date: 2017-10-16 11:51:14
tags: [ssh, tunnel]
---
http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/

```
Host [hostname]
  HostName database.example.com
  IdentityFile ~/.ssh/database.example.com
  LocalForward 9906 127.0.0.1:3306
  User coolio
```

```
ssh -f -N [hostname]
```