# 剑指 Offer 57.1 和为s的两个数字

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

```
示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]

示例 2：
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```

## 提示

- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^6`

# 自己的思路

数组是排序的，因此用双指针分别指向头尾两端，如果当前和大于目标值，则右指针移动，如果小于则左指针移动，找到了就退出。

# 我的代码

```
func twoSum(nums []int, target int) []int {
    i, j := 0, len(nums)-1
    for i < j{
        if nums[i] + nums[j] == target{
            return []int{nums[i],nums[j]}
        }else if nums[i] + nums[j] > target{
            j--
        }else{
            i++
        }
    }
    return []int{-1,-1}
}
```



# 更好的思路



# 规范代码

```go

```

# 思考

这次没啥问题了，比较简单。

