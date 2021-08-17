# docker 安装

## 说明
想要用docker，当然要先安装docker啦！！！  

## 操作
### linux
[说明文档](https://docs.docker.com/engine/install/)  
[centos版](https://docs.docker.com/engine/install/centos/)  
由于用的是centos版，这里以centos版为演示  
```bash
# 先清理原版docker
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 设置环境
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
# 安装docker
sudo yum install docker-ce docker-ce-cli containerd.io
# 启动docker
sudo systemctl start docker
# 安装docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
curl -L https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
cd /usr/local/bin && chmod +x docker-compose
```
### windows
[下载地址](https://docs.docker.com/docker-for-windows/install/)  
在这里直接下载就好了  
注意要设置wsl和wsl引擎  