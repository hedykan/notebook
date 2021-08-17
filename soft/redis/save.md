# redis 数据持久化学习
## 说明
redis是运行与内存的一个数据库，在机器需要停止或更新的时候需要将数据保存下来  

## 使用
[持久化](https://blog.csdn.net/laoluobo76/article/details/108440152)  
```bash
save # save会阻塞整个redis
bgsave # 后台保存，会fork一个子进程专门进行持久化，对redis影响相对较小
save m n # m秒内有n个键变化则保存
config get dbfilename # 查询保存配置名称
```