# acme ssl证书申请学习记录
[中文文档](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)

## http申请
acme会在域名目标目录下放置一个文件访问，如果其服务从外部访问到目标文件，那么即证明目标域名为申请者所拥有，此时则发放证书。  
所有设置好目标域名的目录后再申请十分重要
```bash
docker exec acme.sh  --issue  -d mydomain.com -d www.mydomain.com  --webroot  /home/wwwroot/mydomain.com/
```

## api申请方式
因为用的是dns服务商是阿里云，所有用阿里云的[dnsapi](https://usercenter.console.aliyun.com/#/manage/ak)  
其他厂商的DNS API信息请参考以下[链接](https://github.com/acmesh-official/acme.sh/tree/master/dnsapi)  
[有关dns模式配置](https://blog.csdn.net/jz_youmayfly/article/details/103705335)  
[泛域名配置](https://www.ioiox.com/archives/104.html)  

### 使用docker构建
[docker版构建学习](https://zhuanlan.zhihu.com/p/45425683)
```bash
docker run --rm  -itd  \
  -v "$(pwd)/out":/acme.sh  \
  -e Ali_Key="Ali_Key" \
  -e Ali_Secret="Ali_Secret" \
  --net=host \
  --name=acme.sh \
  neilpang/acme.sh daemon

# 首先
docker exec acme.sh --register-account -m 我的邮箱

# 泛域名证书需要这样申请
docker exec acme.sh --issue --dns dns_ali -d juhuan.store -d '*.juhuan.store'
```

## 证书装载
```bash
# 装载证书
docker exec acme.sh --install-cert -d juhuan.store \
--key-file       /home/www/cert/juhuan.store/key.pem  \
--fullchain-file /home/www/cert/juhuan.store/cert.pem \
--reloadcmd     ""

# nginx添加地址
ssl_certificate /home/www/cert/image.juhuan.store/cert.pem;
ssl_certificate_key /home/www/cert/image.juhuan.store/key.pem;
```