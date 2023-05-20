---
title: LNMP 安裝遇到的問題筆記
date: 2023-05-19 18:11:00
updated: 2023-05-20 18:20:00
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace: ''
  wrap: true
  hljs: true
prismjs:
  enable: true
  preprocess: true
  line_number: true
  tab_replace: ''
categories: 心得
tags: 
- LNMP
- php
- phpMyAdmin
- nginx
- MariaDB
- 2023
---

# LNMP 安裝遇到的問題筆記

由於最近修的資料庫課程作業有要使用 **php + mysql** 完成指定 project。
另外基於我是在 Linux 上做此份作業，[**Aok 電電**](https://github.com/aokblast) 將 LNMP 推薦給我。

* 一點小知識，LNMP 和 LEMP 都有人說，因為中間的 N 指的是 Nginx，且是 Eginx 的發音。

## Outline
1. 安裝流程
2. 一些報錯情況
3. phpMyAdmin in LNMP Stack on Arch Linux


## Arch Linux 上安裝流程
基本上網路上都能夠找到許多教學流程，這裡不贅述。僅放上我有參考的安裝流程網站以供參考。
1. [Arch Linux 服务器安装 LNMP](https://blog.linioi.com/posts/5/)
2. [Install Nginx, MariaDB, PHP7 (LEMP) on Arch Linux Server in 2019](https://www.linuxbabe.com/linux-server/install-lemp-nginx-mariadb-php7-arch-linux-server)

## 一些報錯情況

### 1. types_hash_max_size: 1024 or types_hash_bucket_size: 64; ...


```bash=
$ nginx -t
... [warn] 97437#97437: could not build optimal types_hash, you should increase either types_hash_max_size: 1024 or types_hash_bucket_size: 64; ignoring types_hash_bucket_size
```

我參考了這篇：https://blog.csdn.net/weixin_36349646/article/details/102686936
解法如下：
1. 用 text editor 打開 `/etc/nginx/nginx.conf`
2. 找到 `http {}` 的區域
3. 增加 `types_hash_max_size` 和 `server_names_hash_bucket_size`

```conf=
# /etc/nginx/nginx.conf
...

http {
    types_hash_max_size 4096;
    server_names_hash_bucket_size 128;
    ...
}
...
```
4. 再次測試 `nginx -t`

### 2. [emerg] 103293#103293: open() "/run/nginx.pid" failed (13: Permission denied)

* 這裡先提，修正上面第一個問題後，我 `nginx -t` 時只有報這個錯誤，如下圖：
```bash=
$ nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
2023/05/19 17:29:39 [emerg] 103293#103293: open() "/run/nginx.pid" failed (13: Permission denied)
nginx: configuration file /etc/nginx/nginx.conf test failed
```

* 目前我尚未找到更好的解法，僅在測試時加上 sudo 作為 workaround。
* 這裡上一些有參考過的網站。
    * https://serverfault.com/questions/1042526/open-run-nginx-pid-failed-13-permission-denied
    * https://blog.csdn.net/weixin_42914989/article/details/113791257

## phpMyAdmin in LNMP Stack on Arch Linux

* 如果已經 install nginx 並調校好 `nginx.conf` 基本上從下面這篇文章的 Step 4 開始即可
* [How to Install LEMP Stack on Arch Linux](https://www.linuxtechi.com/install-lemp-stack-on-arch-linux/)