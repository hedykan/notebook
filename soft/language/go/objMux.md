# 对象复用

## 临时对象复用
### sync.Pool
sync.Pool是一种池化手段，通过提供New,Get,Put方法，可以对一些临时对象进行重用，减少内存分配和GC压力。

### 示例
```go
func main() {
    bufferpool := &sync.Pool {
        New: func() interface {} {
            println("Create new instance")
            return struct{}{}
        }
    }

    // 申请
    obj := bufferpool.Get().(struct{})
    // 放回
    defer bufferpool.Put(obj)
    fmt.Println(obj)
}
```

### 使用注意
1. Pool 池里的元素随时可能释放掉，释放策略完全由 runtime 内部管理。
2. Get 获取到的元素对象可能是刚创建的，也可能是之前创建好 cache 住的。使用者无法区分。
3. Pool 池里面的元素个数你无法知道。
4. Pool 的New会并发调用，需要注意New的并发安全问题。

### sync.Pool泛型封装
```go
package utils

import "sync"

type Pool[T Item[T]] struct {
    p sync.Pool
}

type Default[T any] interface {
    Default() T
}

type Reset interface {
    Reset()
}

type Item[T any] interface {
    Reset
    Default[T]
}

func NewPool[T Item[T]]() *Pool[T] {
    return &Pool[T]{
        p: sync.Pool{New: func() any {
            var a T
            return a.Default()
        }},
    }
}

func (p *Pool[T]) Get() T {
    return p.p.Get().(T)
}

func (p *Pool[T]) Put(t T) {
    t.Reset()
    p.p.Put(t)
}

// 原文作者：MaxKing
// 转自链接：https://learnku.com/articles/69662
```

```go
func main() {

    p := utils.NewPool[*Node]()
    fmt.Printf("p.Get(): %v\n", p.Get())

}

type Node struct {
    a int
}

func (n *Node) Reset() {
    n.a = 99
}
func (n *Node) Default() *Node {
    fmt.Printf("n: %v\n", n) // note: n is nil
    return new(Node)
}

// 原文作者：MaxKing
// 转自链接：https://learnku.com/articles/69662
```

## 主动分配内存
### arena
arena是手动分配和释放内存的一种方法，可以在堆内手动固定开辟一段内存空间，在需要的时候释放，可以减轻GC压力。

### 示例
```go
func main() {
    // 创建池
    pool := arena.NewArena()
    // 释放池
    defer pool.Free()

    // 分配一系列单对象
    for i := 0; i < 10; i++ {
        obj := arena.New[int](pool)
        obj = i
        fmt.Println(obj)
    }

    // 或者分配slice 暂时不支持map
    // 参数 mem, length, capacity
    slice := arena.MakeSlice[int](pool, 100, 200)
    slice[0] = 1
    fmt.Println(slice)

    // 不能直接分配string，可借助bytes转换
    src := "hello world"

    bs := arena.MakeSlice[byte](pool, len(src), len(src))
    copy(bs, src)
    str := unsafe.String(&bs[0], len(bs))

    fmt.Println(str)
}
```

### 使用注意
1. arena是当前1.20的实验特性，且暂时加入特性无望，谨慎使用，go1.20及以上通过 `go env -w GOEXPERIMENT=arenas` 并升级gopls使用。
2. 使用arena有可能带来内存泄露问题，使用的时候要十分注意，程序实际上线前，借助一些地址/内存预检测手段检测，常用的就是address sanitizer (asan)/memory sanitizer (msan)。
3. 不要滥用，和对待unsafe, reflect, or cgo一样，只有必要时用。