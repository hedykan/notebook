# docker-compose使用

## 说明
一般一个项目中不止使用一个容器，比如常见的lnmp，都需要nginx，mysql，php-fpm等三个容器进行服务，如果要一个一个启动，是很麻烦的事情。而docker官方提供了一种批量启动的定义脚本，这就是docker-copmpose。

## 定义
docker-compose.yml是定义这一过程的脚本，其由三大块组成，分别是`version`compose版本，`services`服务，和其他定义。  
```yml
version: "3.9"              # 版本

services:                   # 服务项
    nginx:                  # nginx服务
        image: nginx:latest # 派生镜像
        volumes:            # 挂载点
            - ./nginx:/etc/nginx
            - /home/www/:/usr/share/nginx/html
        restart: always     # 启动
        ports:              # 暴露端口
            - "80:80"

    php_fpm:
        # image: php:fpm
        user: "1000:1000"   # 挂载用户id，否则会遇到权限不足的问题
        build: .            # 编译的dockerfile
        volumes: 
            - /home/www/:/var/www/html
        restart: always
```