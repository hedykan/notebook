# go mod 获取私有库

## 通过使用github token以及通过git的insteadOf功能进行获取
1. 先设置go env goprivate `go env -w GOPRIVATE=github.com`
2. github获取到具有访问权限的token，并且设置git config后即可获取到相应私有库内容
`git config --global url."https://${{GITHUB_TOKEN}}@github.com/".insteadOf "https://github.com/"`
3. 删除记录
`git config --global --unset url."https://${{GITHUB_TOKEN}}@github.com/".insteadOf`

### 问题
在`go clean -modcache`后会失效，需要重新申请token并设置

## 通过git clone后手动在go.mod中配置
1. 先`git clone git@github.com:xxx/xxx.git`
2. 在go.mod中编辑
```go.mod
    require github.com/xxx/xxx v0.0.1
    replace github.com/xxx/xxx => ../xxx
```