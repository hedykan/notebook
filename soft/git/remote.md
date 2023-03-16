# 远程仓库
## 新增远程仓库
```bash
# 新建远程地址名称，远程地址
git remote add [remoteName] [remoteAddr]

# 推送所有远程仓库
git push --mirror

# 删除远程仓库
git remote rm [remoteName]
```

## 容器地址
如果使用的是gitea容器，没有配置容器互通的时候的地址为`ssh://git@localhost:222/hedykan/test.git`

可以编辑.ssh/config文件进行多pubkey多账户管理
```bash
# 这里的Host用在 git remote add gitea git@name1.gitea.com:name1/xxx.git
# 这样访问时可以直接访问hostname的地址
Host name1.github.com
    hostname github.com
    Port 22
    User name1
    IdentityFile ~/.ssh/name1_id_rsa
Host name2.github.com 
    hostname github.com
    Port 22
    User name2
    IdentityFile ~/.ssh/name2_id_rsa
Host name1.gitea.com
    hostname gitea.com
    Port 8022
    User name1
    IdentityFile ~/.ssh/name1_id_rsa
```