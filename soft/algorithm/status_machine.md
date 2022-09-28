# 状态机框架

## 说明

## 示例
```go
// 状态机节点
type StatusMachineNode struct {
	StatusId   int // 状态id
	StatusFunc func(in byte) int // 状态转换函数，返回应该转换的状态id
	Handle     func(in byte) // 操作函数，一般为本状态内接收的参数进行运算
}

const (
	nonId = 0
	numId = 1
	strId = 2
)

var machineMap map[int]StatusMachineNode
var buff []int

// 初始化一个字符数字提取状态机
func init() {
	machineMap = make(map[int]StatusMachineNode)
	buff = make([]int, 0)
	nonFunc := func(in byte) int {
		if in >= '0' && in <= '9' {
			return numId
		} else {
			return strId
		}
	}
	nonHandle := func(in byte) {}
    machineMap[nonId] = StatusMachineNode{
		StatusId:   nonId,
		StatusFunc: nonFunc,
		Handle:     nonHandle,
	}

	numFunc := func(in byte) int {
		if in >= '0' && in <= '9' {
			return numId
		} else {
			return strId
		}
	}
	numHandle := func(in byte) {
		if in >= '0' && in <= '9' {
			num, _ := strconv.Atoi(string(in))
			index := len(buff) - 1
			buff[index] = buff[index]*10 + num
		}
	}
    machineMap[numId] = StatusMachineNode{
		StatusId:   numId,
		StatusFunc: numFunc,
		Handle:     numHandle,
	}

	strFunc := func(in byte) int {
		if in < '0' || in > '9' {
			return strId
		} else {
			return numId
		}
	}
	strHandle := func(in byte) {
		if in >= '0' && in <= '9' {
			num, _ := strconv.Atoi(string(in))
			buff = append(buff, num)
		}
	}
	machineMap[strId] = StatusMachineNode{
		StatusId:   strId,
		StatusFunc: strFunc,
		Handle:     strHandle,
	}
}

func main() {
	str := "qwe12weq3w1e23rqq"
	nowNode := machineMap[nonId]
	for _, value := range str {
		// 处理
		nowNode.Handle(byte(value))
		nowStatus := nowNode.StatusFunc(byte(value))
		// 状态转换
		nowNode = machineMap[nowStatus]
	}
	fmt.Println(buff)
}
```