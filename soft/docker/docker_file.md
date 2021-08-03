# dockerfile设置

## 格式
```shell
FROM centos
# 挂载需要指定路径，如/
VOLUME ["/volume1"]
CMD echo "ok"
CMD /bin/bash
```

## 指令
```dockerfile
FROM        # 基础镜像，从此开始构建
MAINTAINER  # 作者，姓名+邮箱
RUN         # 构建需要运行的命令
ADD         # 步骤：添加内容压缩包
WORKDIR     # 镜像工作目录
VOLUME      # 挂载目录
EXPOSE      # 端口配置
CMD         # 指定容器启动时运行的命令，只有最后一个生效
ENTRYPOINT  # 指定容器启动时运行的命令，可以追加命令
ONBUILD     # 构建一个被继承的dockerfile，这时会运行ONBUILD指令。触发指令
COPY        # 类似ADD，将文件拷贝到镜像中
ENV         # 构建时设置环境变量
```