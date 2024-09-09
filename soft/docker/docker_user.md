# docker用户组

默认情况下，docker 命令会使用 Unix socket 与 Docker 引擎通讯。而只有 root 用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 root 用户。因此，更好地做法是将需要使用 docker 的用户加入 docker 用户组。

/*** bash
    $ sudo groupadd docker # 添加docker组
    $ sudo usermod -aG docker $USER # 添加用户进入docker组
    $ sudo newgrp docker # 更新docker用户组
***/
