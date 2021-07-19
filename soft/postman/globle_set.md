# 公共变量设置

## 说明
postman可以设置公共变量以提供参数以及url使用，可以减少工作量以及提高灵活性

## 使用
1. 参数使用  
    `{{key}}`
2. 参数设置
    ```js
    pm.globals.set("url", "test.juhuan.store")
    pm.globals.set("token", pm.response.json().data.token)
    console.log(pm.globals.get("token"))
    ```