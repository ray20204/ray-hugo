---
title: "AWS CodeDeploy"
date: "2021-02-08T08:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
- note
tags:
- aws
- CodeDeploy
---
AWS CodeDeploy 是一個協助開發者部署的服務
<!--more-->
* 特點:
  * Server, serverless, and container applications.
  * Automated deployments
  * Minimize downtime
    * On-Premises 部分可以取得目前可用機器進行更新，方便進行 Auto Scaling
    * 支援 blue/green deployment
  * Stop and roll back
  * Centralized control
    * 集中在 CodeDeploy console or the AWS CLI
  * Easy to adopt
  * Concurrent deployments
* CodeDeploy provides two deployment type options:
  * In-place deployment
  * Blue/green deployment

* 可以自動化部屬到
  * EC2
  * Lambda functions
  * ECS
  * on-premises instances (外部機器)

* AppSpec
  * 設定擋為 yaml 格式 appspec.yml
  ![lifecycle-event-order-in-place](https://docs.aws.amazon.com/codedeploy/latest/userguide/images/lifecycle-event-order-in-place.png)
  * 結構
  ```
  files:
  - source: source-file-location
    destination: destination-file-location
  hooks:
   deployment-lifecycle-event-name:
     - location: script-location
       timeout: timeout-in-seconds
       runas: user-name
  ```

-----
## reference
* [AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html)
* [Deployments on an EC2/On-Premises Compute Platform](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps-server.html)
