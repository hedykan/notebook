# docker网络

## 说明
docker容器不一定只有一个，而如果有多个容器互相协作，他们如何通信呢？  
答案是通过网络通信，而通过网络通信则需要搭建起一个共同的网络，让容器们能够在期间相互通信。

## 使用
### `docker network ls`
查看所有docker网络  

#### 网络模式
1. bridge
2. none
3. host
4. container

### `docker run --link`

### `docker network create`
创建一个网络，在其中的容器均可以通过域名访问。  
```shell
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
docker run --net=mynet nginx # 可以通过这种方式为容器添加网络
```

### `docker network connect`
为已运行的容器添加网络。  
```shell
docker network connect [net_name] [id/name]
```