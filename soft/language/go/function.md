# 奇技淫巧集合

## 为函数添加中间件
```go
type funcMiddle func()

func (f funcMiddle) addMid() funcMiddle {
	return funcMiddle(func() {
		fmt.Println("start")
		f()
		fmt.Println("end")
	})
}

func (f funcMiddle) exec() {
	f()
}

func main() {
    f := func() {
	    fmt.Println("hello world")
	}
	funcMiddle(f).addMid().addMid().exec()
}

// start
// start
// hello world
// end
// end
```