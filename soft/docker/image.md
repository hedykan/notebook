# 镜像相关学习

## 说明
docker中，镜像是派生出容器的母本，操作，封装，拉取镜像实际上就是操作目标副本，软件母本。（还没想好类比）

## 常用命令

### 搜索命令search
这是用来查看目标镜像是否存在的命令，可以查看docker hub是否有目标镜像。  
```shell 
docker search [image]
# 例子
hedykan@DESKTOP-HT7MHUJ:~$ docker search nginx

NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        15210     [OK]
...
```

### 拉取命令pull
这是用来拉取目标镜像的命令，当确认docker hub存在目标镜像后可以通过命令拉取目标镜像。  
```shell
docker pull [image]
# 例子
hedykan@DESKTOP-HT7MHUJ:~$ docker pull nginx

Using default tag: latest
latest: Pulling from library/nginx
33847f680f63: Pull complete
dbb907d5159d: Pull complete
8a268f30c42a: Pull complete
b10cf527a02d: Pull complete
c90b090c213b: Pull complete
1f41b2f2bf94: Pull complete
Digest: sha256:8f335768880da6baf72b70c701002b45f4932acae8d574dedfddaf967fc3ac90
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```