---
title: "Laravel userd Supervisor monitor queue:work process and automatically restart
date: "2018-03-06T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
tags:
- laravel
- PHP
- Supervisor
---
使用 Supervisor 監測 php artisan queue:work process，並自動重啟

<!--more-->

遇到 pods 中 php artisan queue:work 的 process 死掉
導致 jobs 卡住問題
查詢官方文件：
https://laravel.com/docs/5.5/queues#running-the-queue-worker
https://laravel.com/docs/5.5/queues#supervisor-configuration

可以使用 Supervisor 監測 php artisan queue:work process，並自動重啟
先建立 supervisor config 檔案
````
command：執行的 command
autorestart：是否重啟
numprocs：執行 process 的數量
````

    [program:worker]
    process_name=%(program_name)s_%(process_num)02d
    command=php artisan queue:work --timeout=10 --sleep=3                                                                                      autostart=true
    autorestart=true
    numprocs=1
    redirect_stderr=true
    stdout_logfile=/var/log/supervisor/worker.log

修改 docker yaml 檔案，安裝 supervisor 並將 config 放到指定位置

    RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
    RUN apt-get update
    RUN apt-get -y --force-yes install supervisor
    COPY worker/conf/pixbehavior-worker.conf /etc/supervisor/conf.d/

