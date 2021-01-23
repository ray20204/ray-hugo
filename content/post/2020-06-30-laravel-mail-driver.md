---
title: "laravel mail drivers"
date: "2020-06-30T08:00:00+08:00"
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
---
laravel mail drivers
<!--more-->

## Mail driver
* 目前依照官方文件，Mail driver 只能使用一種，在開發上如果需要其他 driver 需要自己新增並且進行替換

### trace 流程
* `AppServiceProvider` 在 boot 時直接使用已經注入的 `TransportManager` 建立一個新的 driver (Swift_Transport)
* MailServiceProvider::registerSwiftMailer
* MailServiceProvider::registerSwiftTransport() 注入 TransportManager 至 app['swift.transport']
之後 Mail 都是透過 TransportManager 產生 Swift_Transport (factory)

* `Manger`：laravel 處理多型 driver 的一個抽象類別
  - $drivers[] 存放已建立的 driver，如果呼叫時不存在，會呼叫 `createDriver($driver)` 建立 driver 並存放至 $drivers
  - $customCreators 存放客制註冊的 driver 及 callback function

* 由 AppServiceProvider 在 boot 時直接使用已經注入的 TransportManager 建立一個新的 driver (Swift_Transport)
就可以使用使用自訂的 mail driver config  `config('mail.test.driver')` 建立新的 mail driver 抽換

`Illuminate\Support\Manger::extend`
```
  /**
     * Register a custom driver creator Closure.
     *
     * @param  string    $driver
     * @param  \Closure  $callback
     * @return $this
     */
    public function extend($driver, Closure $callback)
    {
        $this->customCreators[$driver] = $callback;

        return $this;
    }
    /**
     * Dynamically call the default driver instance.
     *
     * @param  string  $method
     * @param  array   $parameters
     * @return mixed
     */
    public function __call($method, $parameters)
    {
        return $this->driver()->$method(...$parameters);
    }

       /**
     * Get a driver instance.
     *
     * @param  string  $driver
     * @return mixed
     *
     * @throws \InvalidArgumentException
     */
    public function driver($driver = null)
    {
        $driver = $driver ?: $this->getDefaultDriver();

        if (is_null($driver)) {
            throw new InvalidArgumentException(sprintf(
                'Unable to resolve NULL driver for [%s].', static::class
            ));
        }

        // If the given driver has not been created before, we will create the instances
        // here and cache it so we can return it next time very quickly. If there is
        // already a driver created by this name, we'll just return that instance.
        if (! isset($this->drivers[$driver])) {
            $this->drivers[$driver] = $this->createDriver($driver);
        }

        return $this->drivers[$driver];
    }
```
-----
## reference
* [laravel mail](https://laravel.com/docs/5.8/mail)
