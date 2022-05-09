# go 上下文包context

## 核心
```go
// 上下文接口
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```
## 常见使用方法
1. 作为终止通知
    ```go
    ctx, cancel := context.WithCancel(context.Background())
    go func(ctx Context) {
        select {
        case ctx.Done():
            return
        default:
            fmt.Println("do something")
        }
    }()
    // 终止，go程中的ctx.Done()将收到通知
    cancel()
    ```
2. 作为消息传递
    ```go

    ```