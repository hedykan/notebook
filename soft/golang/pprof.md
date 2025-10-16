# Go pprof 性能分析工具

pprof 是 Go 语言内置的性能分析工具，用于可视化和分析应用程序的性能数据。

## 如何使用

在程序中插入指定的包，然后监听指定端口之后，就可以使用 go tool pprof去导出相应的数据。

### 1. HTTP 方式集成 pprof

```go
import (
    _ "net/http/pprof"
    "net/http"
    "log"
)

func main() {
    // 启动 pprof HTTP 服务器
    go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil))
    }()

    // 你的应用代码
    // ...
}
```

### 2. 手动方式启用 pprof

```go
import (
    "os"
    "runtime/pprof"
    "log"
)

func main() {
    // CPU 分析
    cpuFile, err := os.Create("cpu.prof")
    if err != nil {
        log.Fatal("could not create CPU profile: ", err)
    }
    defer cpuFile.Close()

    if err := pprof.StartCPUProfile(cpuFile); err != nil {
        log.Fatal("could not start CPU profile: ", err)
    }
    defer pprof.StopCPUProfile()

    // 你的应用代码
    // ...

    // 内存分析
    memFile, err := os.Create("mem.prof")
    if err != nil {
        log.Fatal("could not create memory profile: ", err)
    }
    defer memFile.Close()

    if err := pprof.WriteHeapProfile(memFile); err != nil {
        log.Fatal("could not write memory profile: ", err)
    }
}
```

### 3. 测试中使用 pprof

```go
import (
    "testing"
    "runtime/pprof"
    "os"
)

func BenchmarkMyFunction(b *testing.B) {
    cpuFile, err := os.Create("bench_cpu.prof")
    if err != nil {
        b.Fatal(err)
    }
    defer cpuFile.Close()

    pprof.StartCPUProfile(cpuFile)
    defer pprof.StopCPUProfile()

    for i := 0; i < b.N; i++ {
        MyFunction()
    }
}
```

## 常用命令

### 获取性能数据

```bash
# CPU 分析（默认30秒）
go tool pprof http://localhost:6060/debug/pprof/profile

# CPU 分析（指定时间）
go tool pprof http://localhost:6060/debug/pprof/profile?seconds=60

# 内存分析
go tool pprof http://localhost:6060/debug/pprof/heap

# 协程分析
go tool pprof http://localhost:6060/debug/pprof/goroutine

# 阻塞分析
go tool pprof http://localhost:6060/debug/pprof/block

# 锁竞争分析
go tool pprof http://localhost:6060/debug/pprof/mutex

# 线程创建分析
go tool pprof http://localhost:6060/debug/pprof/threadcreate
```

### 分析命令

```bash
# 进入交互模式后：
top                    # 显示前10个热点函数
top20                 # 显示前20个热点函数
web                   # 在浏览器中显示调用图
web function_name     # 显示特定函数的调用图
list functionName     # 显示函数的源码和消耗
text                  # 文本格式输出
png                   # 生成PNG图片
svg                   # 生成SVG图片

# 命令行直接生成报告
go tool pprof -text cpu.prof > cpu.txt
go tool pprof -png cpu.prof > cpu.png
go tool pprof -svg cpu.prof > cpu.svg
```

### 高级分析

```bash
# 比较两个性能文件
go tool pprof -base base.prof current.prof

# 查看特定函数调用链
go tool pprof -focus=packageName cpu.prof

# 排除特定函数
go tool pprof -ignore=packageName cpu.prof

# 生成火焰图
go-torch http://localhost:6060/debug/pprof/profile -o torch.svg
```

## 性能分析类型

1. **cpu**: CPU 使用情况分析
2. **heap**: 内存堆使用分析
3. **goroutine**: 协程状态分析
4. **block**: 阻塞操作分析
5. **mutex**: 锁竞争分析
6. **threadcreate**: 线程创建分析

## 导出脚本

创建一个自动化脚本来收集和分析 pprof 数据：

### pprof_collect.sh

```bash
#!/bin/bash

# pprof 数据收集和分析脚本
# 使用方法: ./pprof_collect.sh [hostname] [port] [output_dir]

set -e

# 默认参数
HOST=${1:-"localhost"}
PORT=${2:-"6060"}
OUTPUT_DIR=${3:-"pprof_analysis"}
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")

# 创建输出目录
mkdir -p "$OUTPUT_DIR"

echo "开始收集 pprof 数据..."
echo "目标: $HOST:$PORT"
echo "输出目录: $OUTPUT_DIR"

# 收集不同类型的性能数据
echo "1. 收集 CPU 分析数据 (30秒)..."
curl -s "http://$HOST:$PORT/debug/pprof/profile?seconds=30" > "$OUTPUT_DIR/cpu_${TIMESTAMP}.prof"

echo "2. 收集内存堆分析数据..."
curl -s "http://$HOST:$PORT/debug/pprof/heap" > "$OUTPUT_DIR/heap_${TIMESTAMP}.prof"

echo "3. 收集协程分析数据..."
curl -s "http://$HOST:$PORT/debug/pprof/goroutine" > "$OUTPUT_DIR/goroutine_${TIMESTAMP}.prof"

echo "4. 收集阻塞分析数据..."
curl -s "http://$HOST:$PORT/debug/pprof/block" > "$OUTPUT_DIR/block_${TIMESTAMP}.prof"

echo "5. 收集锁竞争分析数据..."
curl -s "http://$HOST:$PORT/debug/pprof/mutex" > "$OUTPUT_DIR/mutex_${TIMESTAMP}.prof"

echo "6. 收集线程创建分析数据..."
curl -s "http://$HOST:$PORT/debug/pprof/threadcreate" > "$OUTPUT_DIR/threadcreate_${TIMESTAMP}.prof"

# 生成分析报告
echo "生成分析报告..."

# CPU 分析报告
echo "生成 CPU 分析报告..."
go tool pprof -text "$OUTPUT_DIR/cpu_${TIMESTAMP}.prof" > "$OUTPUT_DIR/cpu_${TIMESTAMP}.txt"
go tool pprof -top10 "$OUTPUT_DIR/cpu_${TIMESTAMP}.prof" > "$OUTPUT_DIR/cpu_top10_${TIMESTAMP}.txt"

# 内存分析报告
echo "生成内存分析报告..."
go tool pprof -text "$OUTPUT_DIR/heap_${TIMESTAMP}.prof" > "$OUTPUT_DIR/heap_${TIMESTAMP}.txt"
go tool pprof -top10 "$OUTPUT_DIR/heap_${TIMESTAMP}.prof" > "$OUTPUT_DIR/heap_top10_${TIMESTAMP}.txt"

# 协程分析报告
echo "生成协程分析报告..."
go tool pprof -text "$OUTPUT_DIR/goroutine_${TIMESTAMP}.prof" > "$OUTPUT_DIR/goroutine_${TIMESTAMP}.txt"

# 生成汇总报告
cat > "$OUTPUT_DIR/summary_${TIMESTAMP}.md" << EOF
# pprof 性能分析报告

生成时间: $(date)
目标服务: $HOST:$PORT

## 文件说明

### 性能数据文件
- cpu_${TIMESTAMP}.prof - CPU 分析数据
- heap_${TIMESTAMP}.prof - 内存堆分析数据
- goroutine_${TIMESTAMP}.prof - 协程分析数据
- block_${TIMESTAMP}.prof - 阻塞分析数据
- mutex_${TIMESTAMP}.prof - 锁竞争分析数据
- threadcreate_${TIMESTAMP}.prof - 线程创建分析数据

### 分析报告
- cpu_${TIMESTAMP}.txt - CPU 详细分析报告
- cpu_top10_${TIMESTAMP}.txt - CPU 热点函数 Top 10
- heap_${TIMESTAMP}.txt - 内存详细分析报告
- heap_top10_${TIMESTAMP}.txt - 内存热点函数 Top 10
- goroutine_${TIMESTAMP}.txt - 协程分析报告

## 使用建议

1. 查看 CPU 热点函数:
   \`\`\`bash
   go tool pprof $OUTPUT_DIR/cpu_${TIMESTAMP}.prof
   \`\`\`

2. 查看内存使用情况:
   \`\`\`bash
   go tool pprof $OUTPUT_DIR/heap_${TIMESTAMP}.prof
   \`\`\`

3. 生成可视化调用图:
   \`\`\`bash
   go tool pprof -png $OUTPUT_DIR/cpu_${TIMESTAMP}.prof > cpu_${TIMESTAMP}.png
   \`\`\`

EOF

echo "pprof 数据收集完成！"
echo "结果保存在: $OUTPUT_DIR"
echo "查看汇总报告: $OUTPUT_DIR/summary_${TIMESTAMP}.md"

# 显示 Top 5 CPU 消耗函数
echo ""
echo "=== CPU Top 5 热点函数 ==="
head -10 "$OUTPUT_DIR/cpu_top10_${TIMESTAMP}.txt" 2>/dev/null || echo "无法生成 CPU 报告"

# 显示 Top 5 内存消耗函数
echo ""
echo "=== 内存 Top 5 热点函数 ==="
head -10 "$OUTPUT_DIR/heap_top10_${TIMESTAMP}.txt" 2>/dev/null || echo "无法生成内存报告"
```

### 使用脚本

```bash
# 给脚本添加执行权限
chmod +x pprof_collect.sh

# 使用默认参数（localhost:6060）
./pprof_collect.sh

# 指定目标服务
./pprof_collect.sh 192.168.1.100 8080

# 指定输出目录
./pprof_collect.sh localhost 6060 ./my_pprof_analysis
```

### 依赖检查脚本

```bash
#!/bin/bash
# check_pprof.sh - 检查目标服务的 pprof 端点是否可用

HOST=${1:-"localhost"}
PORT=${2:-"6060"}

echo "检查 $HOST:$PORT 的 pprof 端点..."

# 检查基本连接
if ! curl -s --connect-timeout 5 "http://$HOST:$PORT/debug/pprof/" > /dev/null; then
    echo "❌ 无法连接到 $HOST:$PORT"
    echo "请确保："
    echo "1. 服务正在运行"
    echo "2. 已导入 _ \"net/http/pprof\""
    echo "3. 端口号正确"
    exit 1
fi

echo "✅ pprof 端点可用"

# 检查各个分析类型
profiles=("heap" "goroutine" "block" "mutex" "threadcreate")

for profile in "${profiles[@]}"; do
    if curl -s "http://$HOST:$PORT/debug/pprof/$profile" > /dev/null; then
        echo "✅ $profile 分析可用"
    else
        echo "❌ $profile 分析不可用"
    fi
done

echo ""
echo "开始收集 CPU 分析数据（10秒测试）..."
curl -s "http://$HOST:$PORT/debug/pprof/profile?seconds=10" > /tmp/test_cpu.prof

if [ -s /tmp/test_cpu.prof ]; then
    echo "✅ CPU 分析数据收集成功"
    rm /tmp/test_cpu.prof

    echo ""
    echo "可以使用以下命令开始分析："
    echo "go tool pprof http://$HOST:$PORT/debug/pprof/profile"
else
    echo "❌ CPU 分析数据收集失败"
fi
```

## 最佳实践

- 安装graphviz
- go tool pprof -http=:8080 profile.pprof
- 在生产环境中谨慎使用 pprof
- 定期进行性能基准测试
- 结合 `go test -bench` 进行性能测试
- 使用火焰图等工具可视化性能数据
- 设置合适的采样时间，避免数据量过大

## 参考资源

- [Go pprof 官方文档](https://golang.org/pkg/net/http/pprof/)
- [Profiling Go Programs](https://golang.org/doc/diagnostics/profiling.html)
- [Go-Torch 火焰图工具](https://github.com/uber/go-torch)

*持续更新中...*