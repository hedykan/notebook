# docker数据卷

## 说明
容器的诞生与消失是常有的事情，如果容器在运行时经常产生要保存的数据，或者在启动时想带一点东西进去，那么就需要使用到容器数据卷技术了，其可以看作linux硬链接，指定目标宿主机的目录作为容器的指定目录。

## volumes
```shell
docker -v [source]:[destination]
# 注意：有些只读文件映射后将无法修改，如nginx.conf
# nginx修改配置应该修改/etc/nginx/conf.d/default.conf
```

## 注意
挂载的卷仍属于被挂载目录的用户，如果没有指定用户`--user ${uid}:${gid}`，则后续的操作以及产生的数据会出现权限不足的情况