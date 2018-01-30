---
title: "facebook PHP SDK v5 Exception Sessions are not active"
date: "2018-01-09T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
tags:
- facebook SDK
- PHP
- note
---
Exception Sessions are not active. Please make sure session_start() is at the top of your script.

<!--more-->

trace facebook PHP SDK
先 grep 了 exception message 找到是因為 code 用到的 `FacebookSessionPersistentDataHandler` class 需要用 `session`
所以必須要有 `session_start()`

`/vendor/facebook/graph-sdk/src/Facebook/PersistentData/FacebookSessionPersistentDataHandler.php:51`
會檢查 PHP session 如果不等於開啟狀態會拋出 exception 導致後續無法執行

[ref](https://stackoverflow.com/questions/24194112/facebook-php-sdk-exception-error)
