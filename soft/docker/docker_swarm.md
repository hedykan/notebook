# docker swarm模式

## 如何创建docker swarm
使用`docker swarm init`命令创建
docker-compose.yml中给服务添加
```
    deploy:
      replicas: 2
      update_config:
        parallelism: 1
        delay: 10s
        failure_action: continue
```
使用`docker network create --driver overlay --attachable my-overlay-network`新建overlay网络，并在docker-compos.yml中引用

## 如何部署应用
使用`docker stack deploy -c docker-compose.yml service-name`
使用`docker stack ps service-name`查看服务状态
使用`docker stack rm service-name`删除服务

## 如何更新容器
使用`docker service update --image img-name service-name`更新服务
使用`docker service update --rollback service-name`回滚服务
使用`docker service ps service-name`查看所有服务的容器

## 设置保存副本数
使用`swap swarm update --task-history-limit <limit>`设置保存更新的最大容器数量
