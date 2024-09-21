# 合并有序数组
## 方法
双指针，遍历一遍数组一，如果大于数组二位置，则插入并且数组二指针后移。数组一的指针一直移动，总长度增加

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
	// 双指针，谁小插入进nums1中
	i1, i2 := 0, 0
	nums1 = nums1[:m]

	for i1 <= m-1 && i2 <= n-1 {
		if nums1[i1] >= nums2[i2] {
			// 插入
			nums1 = append(nums1[:i1], append([]int{nums2[i2]}, nums1[i1:]...)...)
			i2 += 1
			m += 1
			fmt.Println("insert", nums1)
		}
		i1 += 1
	}

	// 插入后续
	if i2 <= n {
		nums1 = append(nums1, nums2[i2:]...)
	}
}
```