# 再生服务器

## 说明
wsl重新实现了再生服务器init，无法使用systemd，初始启动和普通ubuntu使用不同。

## 使用
1. 使用`/etc/init.wsl`运行
    进入任意wsl发行版中，创建`/etc/init.wsl`,编辑
    ```shell
    #! /bin/sh
    # $1为第一个指令
    /etc/init.d/cron $1
    service php-fpm $1
    service nginx $1
    ```
2. 使用service启动服务
    ```
    service nginx start
    ```