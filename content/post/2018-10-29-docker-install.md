---
title: "install docker for mysql"
date: "2018-10-29T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
tags:
- docker
---
Install Docker
<!--more-->

* Install Docker：
* Mac:
    - https://store.docker.com/editions/community/docker-ce-desktop-mac
* Windows：
    - https://store.docker.com/editions/community/docker-ce-desktop-windows (Requires Microsoft Windows 10 Professional or Enterprise 64-bit)
    - https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe

* 安裝後執行後系統列會出現鯨魚符號，開啟後會出現下圖，點擊關閉，開啟 Terminal
* ![Mac](https://docs.docker.com/docker-for-mac/images/mac-install-success-docker-cloud.png)

可能會用到指令:

     docker ps -q
     docker ps -a
     docker image list
     docker run
     docker pull
     docker exec -it (id) bash

安裝 MySQL：

     * docker pull mysql
     * docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=1234 mysql
     * docker ps
     * docker exec -it <CONTAINER ID> bash
     // 進入 container 中執行 bash
     * mysql -u root -p

     如果需要額外安裝 phpmyadmin
     * docker pull phpmyadmin/phpmyadmin
     * docker run --name myadmin -d --link mysql:db -p 127.0.0.1:8080:80 phpmyadmin/phpmyadmin
     * http://localhost:8080

* ref：
* [docker-for-mac](https://docs.docker.com/docker-for-mac/install/#install-and-run-docker-for-mac)
* [docker-for-windows](https://docs.docker.com/docker-for-windows/install/#start-docker-for-windows)
