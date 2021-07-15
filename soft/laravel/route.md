# 路由学习

## 说明
这是laravel路由学习的记录

## 路由说明
路由旨在给定一个链接地址，指向一个指定的方法，或函数。  
用以提供外部在同一个域名下使用不同的服务。  

## laravel 路由相关文件
```
./routes/*
```

## 路由查询指令
```
php artisan route:list
```

## 路由书写方法
1. 绝对路由访问  
    例子：
    ```php
    Route::get('/', function() {
        echo "hello world";
    });
    ```

2. 相对路由访问  
    例子：  
    ```php
    // 必传
    Route::get('/{data}', function($data) {
        echo "hello world";
    });
    // 非必传
    Route::get('/{data?}', function($data = NULL) {
        echo "hello world";
    });
    ```

3. 路由群组  
    说明：由于部分路由具有相同的前缀，如：  
    ```
    index/list
    index/get
    ```
    为了方便书写和简化代码，引入了路由群组概念   
    以下代码具有相同前缀，可以为：  
    ```
    index/*
    ```
    例子：  
    ```php
    Route::group(['prefix' => 'index'], function(){
        Route::get('/{data?}', function($data = NULL) {
            echo "hello world";
        });
    })
    ```

4. 路由参数
    说明：可以通过$_GET，$_POST获取uri或form_data参数  
    例子：  
    ```php
    Route::get('/', function() {
        $_GET['id'];
    });
    ```