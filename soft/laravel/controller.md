# 控制器学习

## 说明
这是laravel控制器学习的记录  

## 控制器说明
控制器旨在为应用提供方法，方法将暴露给外部提供具体的服务。  

## 相关文件
```
./app/Http/Controller/*
```

## 控制器建立指令
1. 控制器根目录下创建
    ```
    php artisan make:controller TestController
    ```
2. 控制器下新建目录创建
    ```
    php artisan make:controller Test/TestController
    ```

## 控制器及控制器路由书写方式

1. 控制器中建立方法，并通过'Controller@function'引出
    ```php
    class TestController extends Controller
    {
        public function test(){
            return phpinfo();
        }
    }
    ```
    并在`./routes/web.php`中添加
    ```php
    Route::get('/phpinfo', 'TestController@test');
    ```
    如果非根目录则为  
    ```php
    Route::get('/phpinfo', 'Test\TestController@test');
    ```

注：如果遇到以下这种情况  
```
Target class [TestController] does not exist
```
请在以下文件`./app/Providers/RouteServiceProvider.php`中查找以下代码并解除注释，没有则添加在其中  
```php
class RouteServiceProvider extends ServiceProvider
{
    // protected $namespace = 'App\\Http\\Controllers';
}
```
这一步是使用命名空间为：`./App/Http/Controllers`  