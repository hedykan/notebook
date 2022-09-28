# 组合遍历算法

## 说明
虽然求组合数可以使用计算排列并去重的方式算出来，但是遍历组合如果使用遍历排列并去重则会很慢，时间复杂度为o(n!)，可以使用递归的方式。  

核心思想，每一个组合都由一个值和一个子组合组成，通过树形递归剪枝，可以达到不重复遍历。  
## 示例
```go
// 遍历组合数，核心思想，不放回的抽取所有数字
func get(arr, buff []int, size int) {
	if size == 0 {
        // 打印组合
		fmt.Println(buff)
	}
	length := len(arr) - size + 1
	for i := 0; i < length; i++ { // 每次从数组中取一个出来，用以接下来的组合
		nextBuff := append(buff, arr[0])
		arr = arr[1:]              // 取出第一个后递归取下一个
		get(arr, nextBuff, size-1) // 取下一层
	}
}
```