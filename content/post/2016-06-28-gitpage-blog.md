---
title: "git page blog with jekyll"
date: "2016-06-28"
categories:
- tech
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
coverImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
metaAlignment: center
tags:
- jekyll
- gh-pages
---

build jekyll on github
<!--more-->

### check gem version
* gem -v
* update Gem (sudo)
* gem update --system

### jekyll ###
* [jekyll official](https://jekyllrb.com/docs/installation/)
* use gem install jekyll (sudo)
* `$ gem install jekyll`


     查詢jekyll 版本

    `$ jekyll -v`

    `cd blog folder`

    `jekyll build`

    `jekyll serve`

    [http://localhost:4000](http://localhost:4000)

### git page
push gh-pages branch

`$ git checkout --orphan gh-pages`

`$ git push origin gh-pages`
