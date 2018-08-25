+++
author = "Gary Gong"
categories = ["webprogramming"]
tags = ["nginx", "mysql", "php", "", "", "", ""]
date = 2018-07-31T23:57:00+08:00
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Nginx, MySQL, PHP Installation (Ubuntu 16.04)"
type = "post"

+++

### Install the Nginx Web Server

```
$ sudo apt-get update
$ sudo apt-get install nginx
```

請確認防火牆開啟埠號 80 / 443

<!-- more -->

### Install MySQL

```
$ sudo apt-get install mysql-server
$ mysql_secure_installation
```

請遵照指示文字設定

### Install PHP for Processing

```
$ sudo apt-get install php-fpm php-mysql
```

### Configure the PHP Processor

```
$ sudo vim /etc/php/7.0/fpm/php.ini
```

編輯 php.ini 內部數值，先前請解除註解

```
cgi.fix_pathinfo=0
```

重新啟動 PHP Processor

```
$ sudo systemctl restart php7.0-fpm
```

### Configure Nginx to Use the PHP Processor

```
$ sudo vim /etc/nginx/sites-available/default
```

編輯 default 內部檔案，更動原 server {...}  區塊為下方區塊

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

結束後，利用 nginx -t 檢測是否存在語法錯誤

```
$ sudo nginx -t
```

最後，重新載入 nginx 設定檔案

```
$ sudo systemctl reload nginx
```

