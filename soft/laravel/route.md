# 路由学习

## 说明
这是laravel路由学习的记录

## 路由说明
路由旨在给定一个链接地址，指向一个指定的方法，或函数。  
用以提供外部在同一个域名下使用不同的服务。  

# laravel 路由相关文件
./routes/*  

# 路由书写方法
```php
Route::get('/', function() {
    echo "hello world";
});
```