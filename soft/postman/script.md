# 测试脚本学习

## 说明
postman是基于node.js开发的，其可以使用js代码进行操作

## 脚本使用[文档](https://honye.gitbooks.io/postman/content/scripts/intro_to_scripts.html)
1. 预请求脚本(Pre-request)  
    在请求前可以对请求进行些处理，比如设置下公共请求地址`{{url}}`  
    ```js
    pm.globals.set("url", "test.juhuan.store")
    ```
2. 测试脚本(Test script)  
    在请求后可以对获取的response进行处理，比如获取response.data中的token
    ```js
    pm.globals.set("token", pm.response.json().data.token)
    console.log(pm.globals.get("token"))
    ```