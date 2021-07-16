# 模型学习

## 说明
这是laravel模型的学习记录

## 模型说明
模型是一种将数据，其他集合视为对象的一种方法或操作方式，旨在以面向对象的方式操作数据。

## 相关文件
```
./app/Models/*
```

## 模型建立指令
1. 模型根目录下创建
    ```
    php artisan make:model Table
    ```
2. 模型新建目录创建
    ```
    php artisan make:model Test/Table
    ```

## 模型重要属性
1. 指定表名
    ```php
    // 如果不设置则自动查找tables表
    protected $table = 'table';
    ```
2. 指定主键
    ```php
    protected $primaryKey = 'primaryKey';
    ```
3. 时间戳属性
    ```php
    // 如果为true会往表字段created_at和update_at更新，一般没有
    public $timestamps = false;
    ```
4. 可操作/不可操作字段属性
    ```php
    // 可操作字段
    protected $fillable = ['name', 'age'];
    // 不可操作字段
    protected $guarded = ['id'];
    ```

## 模型操作
1. 增
2. 删
3. 改
4. 查