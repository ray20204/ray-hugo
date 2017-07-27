---
title: "php 輸出excel 可讀 utf8 中文"
date: "2017-07-13T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
coverImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
metaAlignment: center
categories:
- tech
tags:
- php
- excel
- utf8
---

php 輸出可讀 utf8 編碼的 excel
<!--more-->
一開始在輸出文件前方加上BOM 告訴 excel 顯示內容的編碼
查詢 utf-16LE + BOM 可以正確顯示於 windows 及 osx
而分隔的方式是使用 \t 進行分隔，稱為 tsv (Tab-Seperated Values)

	* Charset UTF-16LE (simply UTF-16 was not enough)
	* BOM "\xFF\xFE"
	* \t (tab) as separator
	* Don't forget to encode also separator and CRLFs :-)

ref:
* [How can I output a UTF-8 CSV in PHP that Excel will read properly?](https://stackoverflow.com/questions/4348802/how-can-i-output-a-utf-8-csv-in-php-that-excel-will-read-properly)
* [Which encoding opens CSV files correctly with Excel on both Mac and Windows?](https://stackoverflow.com/questions/6588068/which-encoding-opens-csv-files-correctly-with-excel-on-both-mac-and-windows)
