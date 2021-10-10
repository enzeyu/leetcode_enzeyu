# 剑指 Offer 03. 数组中重复的数字

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```
示例 1：
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

## 提示

- 2 <= n <= 100000

# 自己的思路

能想到的当然是找一个长度为n的map，用于存储每个数字是否出现过，但是显然这样的方法空间复杂度高。

或者对数组进行排序，然后从0到n-2进行索引，当索引和下一个索引对应的值相等时则返回。

# 我的代码

```go
func findRepeatNumber(nums []int) int {
    sort.Slice(nums,func(a,b int) bool{
        return nums[a] < nums[b]
    })
    for i := 0; i < len(nums)-1; i++{
        if nums[i] == nums[i+1]{
            return nums[i]
        }
    }
    return -1
}
```

# 更好的思路

## 原地置换算法

利用了条件所有数字在0~n-1范围内，所以可以保证如果不出现重复数字的话，那么就会nums[i]=i，所以一但不等于的话，我们就可以让nums[nums[i]]与nums[i]进行置换，这样子能够保证nums[nums[i]] = nums[i]，然后继续置换直到在置换前就已经满足nums[nums[i]] = nums[i]。

# 规范代码

```go
// 原地置换
func findRepeatNumber(nums []int) int {
	for i := 0; i < len(nums); i++ {
		for nums[i] != i {
			if nums[nums[i]] == nums[i] {
				return nums[nums[i]]
			}
			nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
		}
	}
	return -1
}
```

# 思考

原地置换代码很巧妙，思路也很nice。

