# 重新规则

# 说明
规则重写是在反向代理过程中常用的功能，在使用过程中经常会出现为了让用户的输入前的uri精简和统一，在输入后在对uri进行加工和重定向。

## 可使用指令
1. set设置指令  
    设置指令可以定义变量，变量以$开头，注意不要与预定义变量冲突  
    ### 使用区域
    1. server块
    2. location块
    3. if块
    ### 使用方式
    ```
    set $key value;
    ```

2. if条件判断指令  
    条件判断指令，可以根据条件判断结果选择不同的nginx设置  
    ### 使用区域
    1. server块
    2. location块
    ### 使用方式
    ```
    if(condition) {
        # 操作
    }
    ```
    ### 特殊参数
    1. -f !-f  
        判断请求的文件是否存在
    2. -d !-d  
        判断请求的目录是否存在使用
    3. -e !-e  
        判断请求的目录或文件是否存在使用
    4. -x !-x  
        判断请求的文件是否可执行

3. rewrite重写指令
    重写指令，可以将获取到的uri进行重写
    ### 使用区域
    1. server块
    2. location块
    3. if块
    ### 使用方式
    ```
    # regex为匹配，replacement为替换
    rewrite ^regex$ replacement [flag];
    # 如果使用括号()表达式，可以用$1, $2在重写的时候使用
    rewrite ^(.*)$ /index.php/$1;
    ```
    ### 注意
    在重写后一定要注意是否能被外部location匹配到，比如  
    ```
    # 重写后为index.php/(.*)
    rewrite ^(.*)$ /index.php/$1;
    location ~ \.php$ {
        # 这个块无法匹配中，因为这里匹配的是(.*)\.php
    }
    location ~ \.php(.*)$ {
        # 这里可以匹配中，因为这里匹配的是(.*)\.php(.*)
    }
    ```

## 常用预定义变量
1. $uri $document_uri  
    当前访问地址的uri，如`http://127.0.0.1/api/`中的`/api/`

2. $args $query_string  
    当前访问地址中的请求参数，如`http://127.0.0.1/api?id=1`中的`id=1`

3. $request_uri  
    当前访问地址的uri，并携带请求参数，如`http://127.0.0.1/api?id=1`中的`api?id=1`

4. $server_name  
    客户端请求到达服务器的名称，如`http://juhuan.store`中的`juhuan.store`

## 预定义变量列表
|参数名|内容|
|---|---|
|$arg_PARAMETER|GET请求中变量名PARAMETER参数的值。|
|$args|这个变量等于GET请求中的参数。例如，foo=123&bar=blahblah;这个变量只可以被修改
|$binary_remote_addr|二进制码形式的客户端地址。|
|$body_bytes_sent|传送页面的字节数|
|$content_length|请求头中的Content-length字段。|
|$content_type|请求头中的Content-Type字段。|
|$cookie_COOKIE|cookie COOKIE的值。|
|$document_root|当前请求在root指令中指定的值。|
|$document_uri|与$uri相同。|
|$host|请求中的主机头(Host)字段，如果请求中的主机头不可用或者空，则为处理请求的server名称(处理请求的server的server_name指令的值)。值为小写，不包含端口。|
|$hostname|机器名使用 gethostname系统调用的值|
|$http_HEADER|HTTP请求头中的内容，HEADER为HTTP请求中的内容转为小写，-变为_(破折号变为|$下划线)，例如：$http_user_agent(Uaer-Agent的值);|
|$sent_http_HEADER|HTTP响应头中的内容，HEADER为HTTP响应中的内容转为小写，-变为_(破折号|$变为下划线)，例如： $sent_http_cache_control, $sent_http_content_type…;|
|$is_args|如果$args设置，值为"?"，否则为""。|
|$limit_rate|这个变量可以限制连接速率。|
|$nginx_version|当前运行的nginx版本号。|
|$query_string|与$args相同。|
|$remote_addr|客户端的IP地址。|
|$remote_port|客户端的端口。|
|$remote_user|已经经过Auth Basic Module验证的用户名。|
|$request_filename|当前连接请求的文件路径，由root或alias指令与URI请求生成。|
|$request_body|这个变量（0.7.58+）包含请求的主要信息。在使用proxy_pass或fastcgi_pass|$指令的location中比较有意义。|
|$request_body_file|客户端请求主体信息的临时文件名。|
|$request_completion|如果请求成功，设为"OK"；如果请求未完成或者不是一系列请求中最后一部分|$则设为空。|
|$request_method|这个变量是客户端请求的动作，通常为GET或POST。包括0.8.20及之前的版本中，这个变量总为main request中的动作，如果当前请求是一个子请求，并不使用这个当前请求的动作。|
|$request_uri|这个变量等于包含一些客户端请求参数的原始URI，它无法修改，请查看$uri更改或重写URI。|
|$scheme|所用的协议，比如http或者是https，比如rewrite ^(.+)$ $scheme://example.com$1  edirect;|
|$server_addr|服务器地址，在完成一次系统调用后可以确定这个值，如果要绕开系统调用，则必须在listen中指定地址并且使用bind参数。|
|$server_name|服务器名称。|
|$server_port|请求到达服务器的端口号。|
|$server_protocol|请求使用的协议，通常是HTTP/1.0或HTTP/1.1。|
|$uri|请求中的当前URI(不带请求参数，参数位于args)，不同于浏览器传递的request_uri的值，它可以通过内部重定向，或者使用index指令进行修改。不包括协议和主机名，例如/foo/bar.html|