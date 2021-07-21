# 反向代理学习

## 说明
nginx在使用过程中最常见的就是反向代理，即接受外部请求，然后将请求分发到相应的端口

## 使用
在server块里定义
1. 通过`proxy_pass`转发
    ```
    location / {
        proxy_pass http://localhost:8000;
    }
    ```