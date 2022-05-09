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