# 容器相关学习

## 说明
容器就是从镜像中派生处理的实例，用运行中的线程作为比喻可能比较好比喻，等我认识更深刻的时候回来更新说明。

## 常用命令

### 运行命令run
运行命令是
### 日志logs
打印控制台展示信息
```shell
docker logs [con_id/con_name]
# 例子
hedykan@DESKTOP-HT7MHUJ:~$ docker logs 35f

[root@35f79be7da45 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@35f79be7da45 /]# cd home/
[root@35f79be7da45 home]# ls
[root@35f79be7da45 home]# touch readme.md
```

### 查看相关配置信息inspect