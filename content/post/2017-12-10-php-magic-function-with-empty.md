---
title: "PHP magic function __get(), __set(), __isset() 與 empty()"
date: "2017-12-10T12:00:00+08:00"
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
coverImage: https://c1.staticflickr.com/5/4301/35965102346_880fdac4bc_h.jpg
metaAlignment: center
categories:
- tech
tags:
- PHP
- magic function
- note
---
PHP magic function __set, __get, __isset 與 empty()

<!--more-->

此次遇到在判斷 empty()  __get() value， // if (empty($this->member)) 判斷永遠為 true 的情形
trace 了一些文章，發現因為 empty() 會執行 isset() 進行判斷
然而在 magic function 中 __get() 的 value isset 會使用 __isset() 進行包裝
所以需要再增加 __isset() method 處理

    <?php
    class Test
    {
        protected $data = [];

        public function __set($key, $value)
        {
             $this->data[$key] = $value;
        }
        
        public function __get($key)
        {
             return $this->data[$key];
        }
    }
    
    $test = new Test();
    $test->member = 'ray';
    var_dump($test->member);  //'ray'
    var_dump(empty($test->member)); //true !! what?
    $member = $test->member;
    var_dump(empty($member)); //false  ????

    class Test
    {
        protected $data = [];

        public function __set($key, $value)
        {
             $this->data[$key] = $value;
        }
        
        public function __get($key)
        {
             return $this->data[$key];
        }

        public function __isset($key)
        {
             return isset($this->data[$key]);
        }
    }
    
    $test = new Test();
    $test->member = 'ray';
    var_dump($test->member);  //'ray'
    var_dump(empty($test->member)); //false
    $member = $test->member;
    var_dump(empty($member)); //false



[ref](http://php.net/manual/en/function.empty.php#93117)
