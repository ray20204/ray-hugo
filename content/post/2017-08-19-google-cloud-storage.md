---
title: "Google Cloud Storage 上傳檔案及下載"
date: "2017-08-19T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
coverImage: //c2.staticflickr.com/2/1704/24265244706_804c38ab62_k.jpg
metaAlignment: center
categories:
- tech
tags:
- GCS
- google cloud
- google cloud storage
---

初次 GCS 上傳及下載檔案
<!--more-->

在 Project 開啟 bucket 後
使用 CLI 將 project 指定至 新建的 project
並使用 `gsutil` 檢視裡面的內容

    gcloud config set project [PROJECT_ID]
    gsutil du -s gs://ticket53247

[getting bucket](https://cloud.google.com/storage/docs/getting-bucket-information)

在基CLI 的操作上非常語意化
上傳就是從本機 cp 至 storage，下載就是從 storage cp 至 local
另外最熟悉的 rm 就是刪除

    gsutil cp [LOCAL_OBJECT_LOCATION] gs://[DESTINATION_BUCKET_NAME]\
    gsutil cp gs://[BUCKET_NAME]/[OBJECT_NAME] [OBJECT_DESTINATION]
    gsutil rm gs://[BUCKET_NAME]/[OBJECT_NAME]

[object-basics](https://cloud.google.com/storage/docs/object-basics)

在上傳至 storage 後，檔案皆有權限管理，如果要開放給外部下載，必須修改權限為 AllUsers

    gsutil acl ch -u AllUsers:R gs://[BUCKET_NAME]/[OBJECT_NAME]

[making-data-public](https://cloud.google.com/storage/docs/access-control/making-data-public)
