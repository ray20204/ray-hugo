---
title: "docker container 連結 localhost MySQL"
date: "2017-07-26T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
coverImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
metaAlignment: center
categories:
- tech
tags:
- docker
- localhost
- MySQL
---

docker connect localhost MySQL
<!--more-->


在本機 docker run 時連接 local 端MySQL HOST 因為 make run 執行 docker run 加更改 DB_HOST

    --env DB_HOST=`ifconfig | grep inet | grep -v inet6 | grep 'broadcast' | cut -d ' ' -f2 | head -n 1`

改使用 en0 ip 使 docker container 可以連接至本機端
但 MySQL 會出現 error message
* [2002] Connection refused
於 ~/.my.cnf  加上

    [mysqld]
    bind-address=0.0.0.0

`error message` 會變成

    [1130] Host '*****' is not allowed to connect to this MySQL server

將 mysql user root host 改為 % 准許從其他 ip 進行登入

mysql command line:

	mysql -u root -p
	mysql>use mysql;
	mysql>update user set host = '%' where user = 'root';


