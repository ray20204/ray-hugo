---
title: "獨立使用 Eloquent"
date: "2021-01-23T08:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
- note
tags:
- php
- laravel
- eloquent
---
單獨使用 eloqeunt
<!--more-->
* `Eloquent` 是一套好用的 ORM，當在其他框架想使用時，找一些方法進行紀錄
* laravel 的 eloquent 元件主要是在 `illuminate/database`，
所以首先是要使用 composer 進行安裝
```
  composer require illuminate/database
```
* 如果要獨立使用的話，首先需要自己處理 database config 的部分，需要實作 `Capsule` 注入自己的 database config (這邊不一定需要使用 `illuminate/config`)

* 官方說明:
```
use Illuminate\Database\Capsule\Manager as Capsule;

$capsule = new Capsule;

$capsule->addConnection([
    'driver'    => 'mysql',
    'host'      => 'localhost',
    'database'  => 'database',
    'username'  => 'root',
    'password'  => 'password',
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
]);

// Set the event dispatcher used by Eloquent models... (optional)
use Illuminate\Events\Dispatcher;
use Illuminate\Container\Container;
$capsule->setEventDispatcher(new Dispatcher(new Container));

// Make this Capsule instance available globally via static methods... (optional)
$capsule->setAsGlobal();

// Setup the Eloquent ORM... (optional; unless you've used setEventDispatcher())
$capsule->bootEloquent();
```

* 有多組 database config 時，只需要再傳入 $name 即可
* source code:
```
  /**
     * Register a connection with the manager.
     *
     * @param  array   $config
     * @param  string  $name
     * @return void
     */
    public function addConnection(array $config, $name = 'default')
    {
        $connections = $this->container['config']['database.connections'];

        $connections[$name] = $config;

        $this->container['config']['database.connections'] = $connections;
    }
```
* 如果有需要支援 `mongo` 的話，可能會裝 `"jenssegers/mongodb"` 這個套件
* 這邊設定就變成
```
$capsule->getDatabaseManager()->extend('mongodb', function ($config) {
    return new Connection($config);
});
```
* 這次主要是在 yii 中使用，所以封裝一個 EloquentServiceProvider 處理

-----
## reference
* [illuminate/database](https://github.com/illuminate/database)
* [jenssegers/mongodb](https://github.com/jenssegers/laravel-mongodb)
