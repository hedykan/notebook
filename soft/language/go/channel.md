# 信道
信道可以看作是一个通讯用的地址  
信道的运行有阻塞式与非阻塞式  
阻塞式在信道未接收到消息时将等待消息  
信道需交由go程填入内容后才可取出，否则将引起恐慌

## 使用
可以往信道里填充信息
```go
func main() {
    c := make(chan struct{})
    go chanTest(c)
    // 直到信道收到才打印
    fmt.Println(<-c)
}

func chanTest(c chan struct{}) {
    c <- struct{}{}
}

```