---
title: "hugo with firebase"
date: "2017-06-24T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
coverImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
metaAlignment: center
categories:
- tech
tags:
- hugo
- firebase
---

build HUGO on firebase
<!--more-->


## HUGO ##
* [HUGO intro](https://gohugo.io/overview/introduction/)
* static website framework
* fast (base on GO lan)
* local serve livereload

### Install
* [HUGO install](https://gohugo.io/overview/installing/)
* MAC OS used homebrew to install

Command Line:

    brew install hugo
    cd work folder
    hugo new site [website_name]
    hugo new post/[article_title].md

* 找一個喜歡的樣板
* [HUGO themes](https://themes.gohugo.io/)
* 樣板頁面有相關教學 Configuration 有許多可設定的地方

* 依據官方案例先使用於 website folder
* `git clone git@github.com:SenjinDarashiva/hugo-uno.git themes/hugo-uno`

`config.toml` 設定語系、名稱、樣板:

    languageCode = "zh-TW"
    title = "test hugo site"
    theme = "hugo-uno"

* `hugo serve`
* http://localhost:1313/ 可預覽成果

### Firebase
* 後端服務平臺（Backend as a Services，BaaS）
* 快速建置
* services：
    * Analytics
    * Authentication
    * Realtime database
    * Cloud Storage (CDN)
    * Hosting
    * Function
    * Test Lab
    * Crash Reporting
    * Performance
    * Notifications
    * Remote Config
    * Dynamic Links
    * AdMob
* [price](https://firebase.google.com/pricing/)

#### Hosting
* HTTPS
* Fast
* Rollback

部署流程：
Firebase CLI (Command Line Interface) 需要先安裝 Node.js

Command Line:

    `npm install -g firebase-tools`
    `firebase login`
    `firebase init`
    `firebase serve` (http://localhost:5000) local preview
    `firebase deploy`
