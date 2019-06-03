---
title: "Drone CI/CD 筆記"
date: "2019-04-24T08:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
- note
tags:
- CI/CD
- PHP
- GKE
---
Drone CI/CD
<!--more-->

* Drone 是一套使用 GO 開發 CI/CD 的工具
使用 yaml 撰寫 pipeline 結合 docker 執行
* 擁有許多 [plugins](http://plugins.drone.io) 可以完成雲端的部署

* Drone 支援 git service 也很完善
	* GitHub
	* GitLab
	* Gitea
	* Gogs
	* Bitbucket Cloud
	* Bitbucket Server

* 在 > 1.0 之後 server 及 agent 已經合成一個 image 不需要額外 compose

* 目前使用的 git server 是 gitea，依照[官方文件](https://docs.drone.io/installation/gitea/single-machine/) 進行架設
* Drone run 起來後，認證會直接吃 Gitea 設定的認證來源
* 把 /etc/localtime:/etc/localtime:ro \ 掛進 volume 的原因是後來在流程中 gcloud 認證碰到時間錯誤的問題

```

	#!/usr/bin/env bash

	DRONE_GITEA_SERVER="https://gitea.tw"
	DRONE_GIT_ALWAYS_AUTH=false
	DRONE_RUNNER_CAPACITY=2
	DRONE_SERVER_PROTO=http
	DRONE_SERVER_HOST="127.0.0.1:8082"
	DRONE_TLS_AUTOCERT=false

	DRONE_GITEA_USERNAME="user"
	DRONE_GITEA_PASSWORD="password"
	PROXY_SERVER="http://proxy:1234"

	docker run \
			--volume=/var/run/docker.sock:/var/run/docker.sock \
			--volume=/etc/localtime:/etc/localtime:ro \
			--volume=/var/lib/drone:/data \
			--env=DRONE_GITEA_SERVER=$DRONE_GITEA_SERVER \
			--env=DRONE_GIT_ALWAYS_AUTH=true \
			--env=DRONE_RUNNER_CAPACITY=2 \
			--env=DRONE_SERVER_HOST=$DRONE_SERVER_HOST \
			--env=DRONE_SERVER_PROTO=$DRONE_SERVER_PROTO \
			--env=DRONE_TLS_AUTOCERT=false \
			--env=DRONE_KEEPALIVE_TIME=20s \
			--env=DRONE_KEEPALIVE_TIMEOUT=20s \
			--env=DRONE_KEEPALIVE_MIN_TIME=5s \
			--env=DRONE_LOGS_DEBUG=true \
			--env=DRONE_LOGS_COLOR=true \
			--env=DRONE_LOGS_PRETTY=true \
			--env=DRONE_GITEA_GIT_USERNAME=$DRONE_GITEA_USERNAME \
			--env=DRONE_GITEA_GIT_PASSWORD=$DRONE_GITEA_PASSWORD \
			--env=http_proxy=$PROXY_SERVER \
			--env=https_proxy=$PROXY_SERVER \
			--restart=always \
			--detach=true \
			--name=drone \
			-p 8082:80 \
			drone/drone:1.1.0

```
yaml 可以吃線上的 secrets，並放進 env 來使用
Drone 也有 CLI 可以在 local 測試，可以先在 local 測試 yaml (但 secrets 需要額外注入，無法使用線上設定)

---
* 執行流程為: (專案為 PHP 部署在 GKE 上)
    * git push => gitea 設定 webhook => Drone 依照 pipeline yaml 執行
    * 預設設定會吃專案底下的 drone.yml
    * 流程為 composer install => phpunit => build image => push image => rolling update

* composer 有遇到 private 套件問題，目前前是使用 composer http auth 暫解，把 key 打進 secrets 裡面
* gcloud/sdk 在使用 ENV 的 GOOGLE_CREDENTIALS 轉成 json file  會有斷行問題，目前是轉成 base64 處理



example：
```

    kind: pipeline
    name: default

    steps:
        - name: backend-composer
          image: composer
          commands:
            - composer install
          when:
            branch:
            - drone

        - name: backend-test
          image: php:7
          commands:
            - cd src/
            - cp .env.example .env
            - ./vendor/bin/phpunit
          when:
            branch:
            - drone
            event:
            - push

        - name: push
          image: plugins/gcr
          settings:
            registry: asia.gcr.io
            repo: test/php7
            dockerfile: web.dockerfile
            tag: latest
            json_key:
              from_secret: GOOGLE_CREDENTIALS
          when:
            branch:
            - drone

        - name: rolling-update
          image: google/cloud-sdk:latest
          environment:
            PROJECT_ID: project_id
            COMPUTE_ZONE: asia-east
            CLUSTER_NAME: cluster_name
            GOOGLE_CREDENTIALS_BASE64:
              from_secret: GOOGLE_CREDENTIALS_BASE64
          commands:
              - gcloud config set project $PROJECT_ID
              - gcloud config set compute/zone $COMPUTE_ZONE
              - echo $GOOGLE_CREDENTIALS_BASE64 | base64 -d > staging_key.json
              - gcloud auth activate-service-account --key-file staging_key.json
              - gcloud container clusters get-credentials $CLUSTER_NAME
              - kubectl patch deployment web -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"date\":\"`date +'%s'`\"}}}}}"
          when:
            branch:
            - drone
```

