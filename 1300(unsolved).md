# 问题

给你一个整数数组 arr 和一个目标值 target ，请你返回一个整数 value ，使得将数组中所有大于 value 的值变成 value 后，数组的和最接近  target （最接近表示两者之差的绝对值最小）。

如果有多种使得和最接近target的方案，请你返回这些整数里的最小值。

请注意，答案不一定是array的数字。

```
示例 1：
输入：arr = [4,9,3], target = 10
输出：3
解释：当选择 value 为 3 时，数组会变成 [3, 3, 3]，和为 9 ，这是最接近 target 的方案。

示例 2：
输入：arr = [2,3,5], target = 10
输出：5

示例 3：
输入：arr = [60864,25176,27249,21296,20204], target = 56803
输出：11361

提示：
1 <= arr.length <= 10^4
1 <= arr[i], target <= 10^5
```

# 自己的思路

问题并不复杂，找到某个区间范围内的正确数字，使得这个数字到其他数字的和最小即可，一层循环即可搞定。问题在于如何确定这个范围。

自己的想法，这个范围应该在1到最大值之间，所以遍历就完事了，经过测试用例的修正，应该把0加上，最后自己的代码超过了时间限制。

# 正确的思路

方法1：枚举+二分查找

value的下界为0，因为当value为0时，数组的和为0。由于target是正整数，因此当value减少的时候，和也会减少，不会比value=0的时候更接近目标值。

value的上界即为最大值。

接下来进行枚举，当枚举到某个值的时候，对小于x的值保持不变，将所有大于x的值变成x。为了实现这样的操作，首先对数组进行排序，然后二分查找，找到最小的比x打的元素arr[i]。此时数组的和是arr[0]+...+arr[i-1]+x*(n-i)。

为了加速求和操作，我们可以预处理出数组 arr 的前缀和，这样数组求和的时间复杂度即能降为 O(1)O(1)。我们将和与 target 进行比较，同时更新答案即可。



方法2：双重二分查找

![image-20200624142255957](C:\Users\Yez\AppData\Roaming\Typora\typora-user-images\image-20200624142255957.png)

# 正确代码

```
//这是自己的代码，思路没问题，但是超过了运行时间限制
func findBestValue(arr []int, target int) int {
    max,arr_sum := find_max_min(arr)
    best_value := 1
    sofar_nearest := arr_sum
    for i:=0;i<=max;i++{
        sum := 0
        for _,j := range arr{
            if j > i{
                sum = sum + i
            }else{
                sum = sum + j
            }
        }
        if(abs(sum-target)) < sofar_nearest{
            sofar_nearest = abs(sum-target)
            best_value = i
        }
    }
    return best_value
}

func find_max_min(arr []int) (int,int){
    sofar_max := arr[0]
    sum := 0
    for _,i := range arr{
        if i > sofar_max{
            sofar_max = i
        }
        sum = sum + i
    }
    return sofar_max,sum
}

func abs(i int) int{
    if i >= 0{
        return i
    }else{
        return i * (-1) 
    }
}

//方法一，源自官方代码
func findBestValue(arr []int, target int) int {
    sort.Ints(arr)
    n := len(arr)
    prefix := make([]int, n + 1)  //前置和数组
    for i := 1; i <= n; i++ {
        prefix[i] = prefix[i-1] + arr[i-1]
    } //求出前置和
    r := arr[n-1]  //r为最大值
    ans, diff := 0, target  //表示当答案是0的时候，最大的差值也就是目标值
    for i := 1; i <= r; i++ {
        index := sort.SearchInts(arr, i) //如果没找到，则返回i在arr里插入的位置
        if index < 0 {//if判断不知道用意是什么，sort.SearchInt返回的应该永远都不会是负数。
            index = -index - 1
        }
        cur := prefix[index] + (n - index) * i
        if abs(cur - target) < diff {
            ans = i
            diff = abs(cur - target)
        }
    }
    return ans
}

func abs(x int) int {
    if x < 0 {
        return -1 * x
    }
    return x
}
```

# 思考

看了其他人的解释，有些人说的单调不减自己没有证明出来，就没有采用。

![image-20200624145431668](C:\Users\Yez\AppData\Roaming\Typora\typora-user-images\image-20200624145431668.png)

