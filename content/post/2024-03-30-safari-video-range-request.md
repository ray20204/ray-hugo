---
title: "Safari video range request"
date: "2024-03-03T08:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
- note
tags:
- Safari
- range request
---
紀錄在處理 safari 影片播放的問題

<!--more-->
* safari 播放影片，資源必須支援 [byte-range requests](https://www.rfc-editor.org/rfc/rfc9110.html#name-byte-ranges)
* step1:
  * 會先送 Range 0-1 跟 server 拿檔案的總長度

* request:
```
Range: bytes=0-1
```
* response:
```
HTTP/1.1 206 Partial Content
...
...
Accept-Ranges: bytes
Content-Length: 2
Content-Range: bytes 0-1/123456
Content-Type: video/mp4
```

* step2:
  * 第二次他會送整段長度的 header，跟 server 取得整段影片，但中間會直接中斷，在使用中斷點作為下一次 request 的開始點，依序直到完成為止，跟 chrome 的機制不一樣， chrome 在第二次會送分段的範圍 ex: 0-6000

* request:
```
Range: bytes=0-123455
```
* response:
```
HTTP/1.1 206 Partial Content
...
...
Content-Length: 123456
Content-Range: bytes 0-123455/123456
```

* 如果 client 與 resource 曾中間還有一層 proxy （可能是檢查登入或是一些商業邏輯)，不可能每次都取完整資源再回傳給 client，所以可以更改 request header 的 Range，自行處理 patial request

```
if (!empty($request->getHeader('Range'))) {
	$httpRange = $request->getHeader('Range')[0];
	$rangeStart = 0;
	$rangeEnd = self::RANGE_BYTES;
	// limit the range to 1000000 bytes
	// bytes=9500-
	if (preg_match("/\w+=(\d+-)$/", $httpRange, $matches)) {
		$rangeStart = (int) $matches[1];
		$rangeEnd = $rangeStart + self::RANGE_BYTES;
	}
	// bytes=9500-10000
	if (preg_match("/\w+=(\d+)-(\d+)/", $httpRange, $matches)) {
		$rangeStart = (int) $matches[1];
		$originalEnd = (int) $matches[2];
		if ($originalEnd - $rangeStart > self::RANGE_BYTES) {
			$rangeEnd = $rangeStart + self::RANGE_BYTES;
		} else {
			$rangeEnd = $originalEnd;
		}
	}
	$request = $request->withHeader('Range', "bytes=$rangeStart-$rangeEnd");
}
```

* Range pattern:
```
# start - end
bytes=0-499
# last 500
bytes=-500
# 從 9500 開始
bytes=9500-

#The first and last bytes only (bytes 0 and 9999):
bytes=0-0,-1

#The first, middle, and last 1000 bytes:
bytes= 0-999, 4500-5499, -1000
```
---
* https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/CreatingVideoforSafarioniPhone/CreatingVideoforSafarioniPhone.html
* https://www.rfc-editor.org/rfc/rfc9110.html#name-byte-ranges
* https://www.stevesouders.com/blog/2013/04/21/html5-video-bytes-on-ios/

