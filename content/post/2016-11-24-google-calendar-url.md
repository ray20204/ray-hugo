---
title: "google calendar url format"
date: "2016-11-24T00:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
coverImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
metaAlignment: center
categories:
- tech
tags:
- google calendar
---

google calendar url query arguments
<!--more-->
## google calendar url 建立活動 ##
~~~
https://www.google.com/calendar/render?action=TEMPLATE
&text=Calendar+Text
&dates=20161207T090000/20161207T110000
&details=this+is+detail
&ctz=Asia/Taipei
&location=Taipei
&trp=false
&sf=true
&src=your.calendar.email+account
~~~
~~~
'text'     => 標題,
'dates'    => 時間範圍,
'details'  => 說明,
'ctz'      => 時區,
'location' => 地點,
'trp'      => 是否忙碌,
'sf'       => 是否視訊通話,
'src'      => 日曆
~~~
