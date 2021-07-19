# 中间件学习

## 说明
这是laravel中间件的学习记录  

## 中间件说明
在http请求的开始或者结束的时候，有些公共的处理，中间件就是在这些时候给request进行处理的程序。  

## 相关文件
`/app/Http/Middleware/*`

## 中间件建立指令
`php artisan make:middleware middleware`

## 中间件使用方式
说明：  
`$next`是前面传入的函数，如共有三个函数，`$first`, `$second`, `$third`,则前置后置的意思其实是  
```php
$first($second($third()));

function first() {
    // 操作
    $second();
}

function third() {
    $second();
    // 操作
}
```
1. 中间件前置操作
    ```php
    // 操作
    return $next($request);
    ```
2. 中间件后置操作
    ```php
    $response = $next($request);
    // 操作
    return $response;
    ```

## 注意
`$response`的操作注意是否有getData，可以通过`method_exists()`来判断