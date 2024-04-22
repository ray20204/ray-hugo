---
title: "Laravel Octane Note"
date: "2022-08-18T08:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
- note
tags:
- Laravel
---
Laravel Octane
<!--more-->
* 瓶頸
  * I/O 效能瓶頸，Laravel 本身由多個 components 組成，載入的檔案相當多，造成 I/O 的消耗，所以有些人會選擇輕量化的 lumen
  * PHP 原生不支援以常駐在系統記憶體的方式執行，每次執行都需要新的生命週期

* Octane 透過 Swoole or RoadRunner 提高程式啟動效率，並與 PHP OPcache 結合使用，啟動後，會常駐 memory 加速，減少重複的 import 及編譯，另外可以 fork 多個 worker 處理
  * 等於 Nginx + php-fpm 的角色

* swoole
* openswoole
  * It's more than NGINX
  * it doesn't need any RPC buses
  * it's written in C
  * gives you access to low level API, such as shared memory tables..etc ( RoadRunner can only dream of this )
  * 相關 lib 可能不支援，需要進行盤點
* RoadRunner
  * Go
  * NGNIX replacement,
  * RPC bus and KV system,
  * doesn’t touches PHP code, extensions, or the runtime.
  * 既有專案導入較快

## Warning
* 同一個 wokrer 會使用同一個 container instance，需要額外注意 singleton
* https://laravel.com/docs/9.x/octane#dependency-injection-and-octane


-----
* https://www.reddit.com/r/laravel/comments/nbc742/laravel_octane_roadrunner_or_swoole_pros_and_cons/

