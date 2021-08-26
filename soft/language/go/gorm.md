# gorm使用说明

## 安装gorm
使用gorm.io提供的轮子  
```bash
# 安装gorm
go get -u gorm.io/gorm
# 安装驱动
go get -u gorm.io/driver/postgres
```

## 使用
要使用gorm需要先引入  
```go
import (
	"gorm.io/driver/postgres"
	"gorm.io/gorm"
)
```

引入后，建立连接  
```go
	dsn := "host=localhost user=postgres password=postgres dbname=go_test port=5432 sslmode=disable TimeZone=Asia/Shanghai"
	db, _ := gorm.Open(postgres.Open(dsn), &gorm.Config{})
```