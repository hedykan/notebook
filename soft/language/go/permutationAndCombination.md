# 排列组合算法
这里列出了常用的排列组合算法，以便以后使用。

有时候遇上自己数学能力不足，暂时没办法通过计算的方式的时候，可以来用用傻办法，直接通过列出所有组合，再结合过滤器，计算一些概率。

## 具体
```go
// 排列数量算法
func P(m, n int64) int64 {
	if n == 0 {
		return 1
	}
	sum := m
	for i := int64(1); i < n; i++ {
		sum *= m - i
		// sum.Mul(sum, big.NewInt(num - i))
	}
	return sum
}

// 组合数量算法
func C(m, n int64) int64 {
	if n == 0 {
		return 1
	}
	return P(m, n) / P(n, n)
}

type PCer interface{}

type PCerArr []PCer

// 遍历组合数，核心思想，不放回的抽取所有数字
func GetCombination(arr, buff PCerArr, size int, proc func(buff []PCer)) {
	if size == 0 {
		proc(buff)
		return
	}
	length := len(arr) - size + 1
	for i := 0; i < length; i++ { // 每次从数组中取一个出来，用以接下来的组合
		nextBuff := append(buff, arr[0])
		arr = arr[1:]                               // 取出第一个后递归取下一个
		GetCombination(arr, nextBuff, size-1, proc) // 取下一层
	}
}

```